FROM mirrors.tencent.com/blueking/python:3.6.14-ts3
USER root

ENV LC_ALL=C.UTF-8 \
    LANG=C.UTF-8

RUN mkdir -p ~/.pip && \
    printf '[global]\nindex-url = https://mirrors.tencent.com/pypi/simple/' > ~/.pip/pip.conf && \
    dnf install -y gcc gcc-c++ gettext && \
    pip install --upgrade pip && \
    pip install poetry==1.1.13 && \
    poetry config repositories.pypi https://mirrors.tencent.com/pypi/simple/ && \
    dnf install -y libtool-ltdl-devel xmlsec1-devel xmlsec1-openssl-devel && \
    pip install python3-saml==1.14.0 xmlsec==1.3.13

WORKDIR /app
COPY src/login/pyproject.toml /app
COPY src/login/poetry.lock /app
RUN poetry config virtualenvs.create false && poetry install --no-dev

COPY src/login/wsgi.py /app
COPY src/login/bklogin /app/bklogin
COPY src/login/locale /app/locale
COPY src/login/static /app/static
COPY src/login/manage.py /app
COPY src/bkuser_global /app/bkuser_global
COPY src/login/bin/start.sh /app

CMD ["bash", "/app/start.sh"]