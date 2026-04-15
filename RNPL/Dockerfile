FROM mageia:9
USER root

#Environment Variables
ENV F77="gfortran"
ENV F77FLAGS="-O6 -fno-second-underscore"
ENV F77CFLAGS="-c"
ENV F77LFLAGS="-L/usr/local/lib"
ENV LIBBLAS="-lblas"
ENV CC="gcc"
ENV CCFLAGS="-O3"
ENV CCCFLAGS="-I/usr/local/include -I/usr/include/X11 -c"
ENV CCLFLAGS="-L/usr/local/lib -L/usr/lib/X11"
ENV CXX="gcc"
ENV CXXFLAGS="-O"
ENV CXXCFLAGS="-I/usr/local/include -I/usr/include/X11 -c"
ENV CXXLFLAGS="-L/usr/local/lib -L/usr/lib/X11"
ENV RNPL_RNPL="rnpl"
ENV RNPL_F77="gfortran -O6 -fno-second-underscore -fallow-argument-mismatch -w"
ENV RNPL_F77LOAD="gfortran -O6 -fno-second-underscore -fallow-argument-mismatch -w -L/usr/local/lib"
ENV RNPL_F77PP="touch"
ENV RNPL_FLIBS="-lrnpl  -lxvs"
ENV RNPL_CC="gcc -O3 -I/usr/local/include -I/usr/local/intel/include -I/usr/include/X11"
ENV RNPL_CCLOAD="gcc -O3 -L/usr/local/lib -L/usr/lib/X11"
ENV RNPL_CLIBS=-"lrnpl -lxvs -lm"
ENV LD_LIBRARY_PATH="/lib:/usr/lib:/usr/local/lib"

RUN dnf install -y \
    nano \
    vim \
    wget \
    htop \
    ffmpeg \
    gcc \
    gcc-gfortran \
    imake \
    gccmakedep \
    make \
    perl \
    flex \
    bison \
    lib64tiff-static-devel \
    lib64jpeg-static-devel \
    lib64x11-devel \
    lib64xpm-devel \
    lib64mesagl-devel \
    lib64mesaglu1-devel \
    && dnf clean all

#Install Helvetica Fonts
WORKDIR /tmp
RUN wget http://laplace.physics.ubc.ca/Doc/rnpletal/Helvetica.ttf.gz
RUN gunzip Helvetica.ttf.gz 
RUN mkdir -p /usr/share/fonts/truetype/myfonts
RUN mv Helvetica.ttf /usr/share/fonts/truetype/myfonts/
RUN fc-cache -f -v /usr/share/fonts/truetype/myfonts

#Install RNPL
WORKDIR /tmp
RUN wget ftp://laplace.physics.ubc.ca/pub/rnpletal/rnpletal.tar.gz
RUN tar zxf rnpletal.tar.gz
WORKDIR /tmp/rnpletal
RUN ./Install.gnu /usr/local

#Install XForms
WORKDIR /tmp
RUN wget ftp://laplace.phas.ubc.ca/pub/xforms-1.0/xforms-1.0.tar.gz
RUN tar zxf ./xforms-1.0.tar.gz
WORKDIR /tmp/xforms-1.0
RUN xmkmf -a && make install

#Install XVS
WORKDIR /tmp
RUN wget ftp://laplace.physics.ubc.ca/pub/xvs/xvs.tar.gz
RUN tar zxf xvs.tar.gz
WORKDIR /tmp/xvs
RUN ./configure --prefix=/usr/local && make install

# #Install DV
WORKDIR /tmp
RUN wget ftp://laplace.physics.ubc.ca/pub/DV/DV.tar.gz
RUN tar zxf DV.tar.gz
WORKDIR /tmp/DV
RUN ./configure --prefix=/usr/local && make install

#Install julia
# RUN curl -fsSL https://install.julialang.org | sh

WORKDIR /PROJECT

CMD ["bash"]