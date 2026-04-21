FROM ubuntu:14.04

RUN apt-get update && apt-get install -y \
    apache2 \
    curl \
    wget \
    build-essential


RUN wget https://ftp.gnu.org/gnu/bash/bash-4.1.tar.gz && \
    tar xzf bash-4.1.tar.gz && \
    cd bash-4.1 && \
    ./configure && make && make install


RUN a2enmod cgi


RUN mkdir -p /usr/lib/cgi-bin


RUN echo '#!/usr/local/bin/bash' > /usr/lib/cgi-bin/test.cgi && \
    echo 'echo "Content-type: text/plain"' >> /usr/lib/cgi-bin/test.cgi && \
    echo 'echo ""' >> /usr/lib/cgi-bin/test.cgi && \
    echo 'echo "Hello from CGI"' >> /usr/lib/cgi-bin/test.cgi && \
    chmod +x /usr/lib/cgi-bin/test.cgi


RUN printf '<Directory /usr/lib/cgi-bin>\n    Options +ExecCGI\n    AddHandler cgi-script .cgi\n</Directory>\n' >> /etc/apache2/apache2.conf

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"] 