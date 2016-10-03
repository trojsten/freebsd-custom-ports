# $FreeBSD$

PORTNAME=	uwsgi
PORTVERSION=	2.0.13
PORTREVISION=	1
CATEGORIES=	www python
MASTER_SITES=	http://projects.unbit.it/downloads/
CONFLICTS_INSTALL= uwsgi

MAINTAINER=	demon@FreeBSD.org
COMMENT=	Developer-friendly WSGI server which uses uwsgi protocol

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/LICENSE

OPTIONS_DEFINE= DEBUG JSON PCRE XML PLUG_CGI PLUG_JVM PLUG_LUA PLUG_PHP PLUG_PYTHON27 PLUG_PYTHON34 PLUG_PYTHON35 PLUG_RACK PLUG_PSGI

USES=		        python ssl gettext-runtime
PLUG_LUA_USES=          lua
PLUG_PHP_USES=          php:embed,build
PLUG_PSGI_USES=         perl5
USE_PYTHON=	        distutils
USE_RC_SUBR=	        uwsgi
USE_PHP=                session:build

PLUG_CGI_DESC= Build the CGI plugin
PLUG_JVM_DESC= Build the JVM plugin
PLUG_LUA_DESC= Build the LUA plugin
PLUG_PHP_DESC= Build the PHP plugin
PLUG_PYTHON27_DESC= Build the Python 2.7 plugin
PLUG_PYTHON34_DESC= Build the Python 3.4 plugin
PLUG_PYTHON35_DESC= Build the Python 3.5 plugin
PLUG_RACK_DESC= Build the Rack plugin
PLUG_PSGI_DESC= Build the PSGI plugin

DEBUG_VARS=             PYDISTUTILS_BUILDARGS+=--debug

BUILD_DEPENDS=               python:lang/python
PLUG_PYTHON27_BUILD_DEPENDS= python2.7:lang/python27
PLUG_PYTHON34_BUILD_DEPENDS= python3.4:lang/python34
PLUG_PYTHON35_BUILD_DEPENDS= python3.5:lang/python35

JSON_LIB_DEPENDS=       libjansson.so:devel/jansson
PCRE_LIB_DEPENDS=       libpcre.so:devel/pcre
XML_LIB_DEPENDS=        libxml2.so:textproc/libxml2

MAKE_ENV+=      CPUCOUNT=${MAKE_JOBS_NUMBER}
MAKE_ENV+=      UWSGI_PROFILE=nolang

PYSETUP=                        uwsgiconfig.py
PYDISTUTILS_BUILD_TARGET=       --build 
PYDISTUTILS_BUILDARGS=          --verbose

PLIST_FILES=               bin/uwsgi \
		           %%PYTHON_SITELIBDIR%%/uwsgidecorators.py
PLUG_CGI_PLIST_FILES=      lib/uwsgi/cgi_plugin.so
PLUG_JVM_PLIST_FILES=      lib/uwsgi/jvm_plugin.so \
			   ${JAVAJARDIR}/uwsgi.jar
PLUG_LUA_PLIST_FILES=      lib/uwsgi/lua_plugin.so
PLUG_PHP_PLIST_FILES=      lib/uwsgi/php_plugin.so
PLUG_PYTHON27_PLIST_FILES= lib/uwsgi/python27_plugin.so
PLUG_PYTHON34_PLIST_FILES= lib/uwsgi/python34_plugin.so
PLUG_PYTHON35_PLIST_FILES= lib/uwsgi/python35_plugin.so
PLUG_RACK_PLIST_FILES=     lib/uwsgi/rack_plugin.so
PLUG_PSGI_PLIST_FILES=     lib/uwsgi/psgi_plugin.so

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MJSON}
O_JSON=         jansson
.else
O_JSON=         false
.endif

.if ${PORT_OPTIONS:MPCRE}
O_PCRE=         true
.else
O_PCRE=         false
.endif

.if ${PORT_OPTIONS:MXML}
O_XML=          libxml2
.else
O_XML=          false
.endif

.if ${PORT_OPTIONS:MPLUG_JVM}
USE_JAVA=       yes
JAVA_BUILD=     yes
JAVA_RUN=       yes
.endif

.if ${PORT_OPTIONS:MPLUG_RACK}
USE_RUBY=       yes
.endif

post-patch:
	${REINPLACE_CMD} -e s#@JSON@#${O_JSON}# -e s#@XML@#${O_XML}# -e s#@PCRE@#${O_PCRE}# ${WRKSRC}/buildconf/base.ini
	${REINPLACE_CMD} -e "s#plugin_dir = .#plugin_dir = /usr/local/lib/uwsgi#" ${WRKSRC}/buildconf/base.ini

do-configure:
	@${DO_NADA}

post-build-PLUG_CGI-on:
	cd ${WRKSRC} && ./uwsgi --build-plugin "plugins/cgi cgi"

post-build-PLUG_JVM-on:
	cd ${WRKSRC} && ./uwsgi --build-plugin "plugins/jvm jvm"

post-build-PLUG_LUA-on:
	cd ${WRKSRC} && UWSGICONFIG_LUAINC=${LUA_INCDIR} UWSGICONFIG_LUALIBPATH=${LUA_LIBDIR} UWSGICONFIG_LUALIB=lua-${LUA_VER} ./uwsgi --build-plugin "plugins/lua lua"

post-build-PLUG_PHP-on:
	cd ${WRKSRC} && ./uwsgi --build-plugin "plugins/php php"

post-build-PLUG_RACK-on:
	cd ${WRKSRC} && ./uwsgi --build-plugin "plugins/rack rack"

post-build-PLUG_PSGI-on:
	cd ${WRKSRC} && ./uwsgi --build-plugin "plugins/psgi psgi"

post-build-PLUG_PYTHON27-on:
	cd ${WRKSRC} && PYTHON=python2.7 ./uwsgi --build-plugin "plugins/python python27"

post-build-PLUG_PYTHON34-on:
	cd ${WRKSRC} && PYTHON=python3.4 ./uwsgi --build-plugin "plugins/python python34"

post-build-PLUG_PYTHON35-on:
	cd ${WRKSRC} && PYTHON=python3.5 ./uwsgi --build-plugin "plugins/python python35"

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/${PORTNAME} ${STAGEDIR}${PREFIX}/bin/
	${MKDIR} ${STAGEDIR}${PYTHONPREFIX_SITELIBDIR}
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/uwsgi
	${INSTALL_DATA} ${WRKSRC}/uwsgidecorators.py ${STAGEDIR}${PYTHONPREFIX_SITELIBDIR}

do-install-PLUG_CGI-on:
	${INSTALL_LIB} ${WRKSRC}/cgi_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_JVM-on:
	${INSTALL_LIB} ${WRKSRC}/jvm_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/
	${INSTALL_DATA} ${WRKSRC}/plugins/jvm/uwsgi.jar ${STAGEDIR}${JAVAJARDIR}/uwsgi.jar

do-install-PLUG_LUA-on:
	${INSTALL_LIB} ${WRKSRC}/lua_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_PHP-on:
	${INSTALL_LIB} ${WRKSRC}/php_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_RACK-on:
	${INSTALL_LIB} ${WRKSRC}/rack_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_PSGI-on:
	${INSTALL_LIB} ${WRKSRC}/psgi_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_PYTHON27-on:
	${INSTALL_LIB} ${WRKSRC}/python27_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_PYTHON34-on:
	${INSTALL_LIB} ${WRKSRC}/python34_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/

do-install-PLUG_PYTHON35-on:
	${INSTALL_LIB} ${WRKSRC}/python35_plugin.so ${STAGEDIR}${PREFIX}/lib/uwsgi/


.include <bsd.port.mk>
