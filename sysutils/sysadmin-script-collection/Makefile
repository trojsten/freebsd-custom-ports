# Created by: Kamila Souckova <kamila@ksp.sk>
# $FreeBSD$

PORTNAME=	sysadmin-script-collection
PORTVERSION=	1.0.20160326
CATEGORIES=	sysutils

MAINTAINER=	kamila@ksp.sk
COMMENT=	Collection of scripts that simplify maintenance of our servers

LICENSE=	MIT

RUN_DEPENDS=	git:${PORTSDIR}/devel/git \
		python3:${PORTSDIR}/lang/python3

USE_GITHUB=	yes
GH_ACCOUNT=	trojsten
GH_TAGNAME=	cd2d5bdf068c80866627db904170d46e26677f38

NO_ARCH=	yes
NO_BUILD=	yes

BIN_FILES=	properly/properly \
		metadata-git-wrapper/mgit metadata-git-wrapper/mgit-init \
		sudo-git-wrapper/sgit \
		zfs-shell/zfs-shell \
		repo-dirty-checker/repo-dirty-checker \
		setup-log/setup-log

ETC_FILES=	properly/properly.conf \
		repo-dirty-checker/repo-dirty-checker.conf \
		setup-log/setup-log.conf

post-patch:
.for _file in ${BIN_FILES}
	@${REINPLACE_CMD} -e "s|/usr/local/|${PREFIX}/|g" \
		${WRKSRC}/${_file}
.endfor

do-install:
.for _file in ${BIN_FILES}
	${INSTALL_SCRIPT} ${WRKSRC}/${_file} ${STAGEDIR}${PREFIX}/bin/${_file:T}
.endfor
.for _file in ${ETC_FILES}
	${INSTALL_SCRIPT} ${WRKSRC}/${_file} ${STAGEDIR}${PREFIX}/etc/${_file:T}.sample
.endfor

.include <bsd.port.mk>
