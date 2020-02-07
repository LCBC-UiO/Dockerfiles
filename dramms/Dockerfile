# Use CentOS 7
FROM fsl:6.0.1

RUN yum groupinstall -y 'Development Tools'
RUN yum install -y cmake \
		   zlib-devel

COPY dramms-1.5.1-source.tar.gz /tmp/.
RUN cd /tmp && tar -xvf dramms-1.5.1-source.tar.gz && cd dramms-1.5.1-source/build && cmake -D CMAKE_INSTALL_PREFIX=/opt/dramms/ . && make -j5 

CMD ["/bin/bash"]
