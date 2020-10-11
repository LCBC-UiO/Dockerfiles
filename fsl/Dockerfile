# Dockerfile for building fsl from source
# Build with CUDA 10.1

FROM nvidia/cuda:10.1-devel-centos7

ENV FSLDIR="/opt/fsl-6.0.4" \
    PATH="/opt/fsl-6.0.4/bin:$PATH"

RUN yum install -y -q \
    epel-release

RUN yum install -y -q \
           bc \
           file \
           libGL \
           libGLU-devel \
           libICE \
           libSM \
           libX11 \
           libXcursor \
           libXext \
           libXft \
           libXinerama \
           libXrandr \
           libXt-devel \
           libgomp \
           libjpeg \
           libmng \
           libpng12 \
           sudo \
           wget \
           openblas-devel \
           expat-devel \
           libX11-devel \
           mesa-libGL-devel \
           zlib-devel \
           cmake \
    && yum group install -y -q "Development Tools" \
    && yum clean packages \
    && rm -rf /var/cache/yum/* 

RUN echo "Downloading VTK ..." \
    && mkdir -p /tmp/vtk-7.0.0 \
    && curl -fsSL --retry 5 https://github.com/Kitware/VTK/archive/v7.0.0.tar.gz \
    | tar -xz -C /tmp/vtk-7.0.0 --strip-components 1 \
    && echo "Building VTK from source ..." \
    && cd /tmp/vtk-7.0.0 \
    && cmake -D CMAKE_INSTALL_PREFIX=/opt/vtk-7.0.0 . \
    && make -j5 && make install && rm -rf /tmp/vtk-7.0.0

RUN echo "Downloading FSL ..." \
    && mkdir -p /opt/fsl-6.0.4 \
    && curl -fsSL --retry 5 https://fsl.fmrib.ox.ac.uk/fsldownloads/fsl-6.0.4-sources.tar.gz \
    | tar -xz -C /opt/fsl-6.0.4 --strip-components 1

# Apply patch by Caspar van Leeuwen, reported in the FSL mail archives 
# (https://www.jiscmail.ac.uk/cgi-bin/webadmin?A2=FSL;2b74023.2006)
RUN echo "Applying patch ..." \
    && sed -i 's;device_vector.h;thrust/device_vector.h;g \
               s;host_vector.h;thrust/host_vector.h;g' /opt/fsl-6.0.4/src/fdt/CUDA/*.cu* \
    && sed -i "s;-I\${INC_CUDA}/thrust;;g" src/fdt/Makefile

RUN echo "Building FSL ..." \
    && cd /opt/fsl-6.0.4/ \
    && sed -i "s;9.1;10.1;g" config/buildSettings.mk \
    && sed -i "s;/opt/cuda-;/usr/local/cuda-;g" config/buildSettings.mk \
    && sed -i "s;/Users/cowboy/VTK7/include/vtk-7.0;/opt/vtk-7.0.0/include/vtk-7.0;g" config/buildSettings.mk \
    && sed -i "s;/home/fs0/cowboy/var/caper_linux_64-gcc4.4/VTK7/include/vtk-7.0;/opt/vtk-7.0.0/include/vtk-7.0;g" config/buildSettings.mk \
    && sed -i "s;/home/fs0/cowboy/var/caper_linux_64-gcc4.4/VTK7/lib;/opt/vtk-7.0.0/lib;g" config/buildSettings.mk \
    && sed -i "s;VTKSUFFIX = -7.0;VTKSUFFIX = -7.0;g" config/buildSettings.mk \
    && sed -i "s;/Users/cowboy/VTK7/lib;/opt/vtk-7.0.0/lib;g" config/buildSettings.mk \
    && sed -i "s;${LIBRT};${LIBRT} -Wl,-rpath,${VTKDIR_LIB};g" src/mist/Makefile \
    && ./build 

RUN ln -s /opt/fsl-6.0.4/bin/eddy_cuda10.1 /opt/fsl-6.0.4/bin/eddy_cuda