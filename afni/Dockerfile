# Use Centos 7
FROM centos:7.7.1908

# Install according to build instructions on 
# https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/background_install/install_instructs/steps_linux_Fed_RH.html#install-prerequisite-packages

RUN yum -y update && yum -y upgrade
RUN yum install -y tcsh libXp openmotif gsl xorg-x11-fonts-misc       \
                    PyQt4 R-devel netpbm-progs gnome-tweak-tool ed     \
	            xorg-x11-server-Xvfb firefox && yum update -y

# Download and install AFNI
RUN cd /tmp && curl -O https://afni.nimh.nih.gov/pub/dist/bin/linux_ubuntu_16_64/@update.afni.binaries && tcsh @update.afni.binaries -package linux_centos_7_64 -do_extras -bindir /opt/afni

CMD ["/bin/bash"]
