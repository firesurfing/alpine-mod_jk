# docker alpine mod_jk
I have built mod_jk in alpine 3.4(base on tomcat-connectors-1.2.42-src), so you can download it directly(https://github.com/firesurfing/alpine-mod_jk/raw/master/mod_jk/mod_jk.so).

The following content is my roadmap to solve this problem.

apache offical alpine image（https://github.com/docker-library/httpd/blob/a5ab6fb434c13a3578a2e7e6ce2cf30d66c625bc/2.2/alpine/Dockerfile） provides common way to build apache2 in alpine, however when I tried to add mod_jk.so, I encountered error about glibc as below:

    "__strtok_r: symbol not found"

The reason is the mod_jk.so we download from offical web was not built in alpine. So I began to download mod_jk's source code and build it in alpine's container. However I encountered another problem about glibc as below:

    fatal error: sys/socketvar.h: No such file or directory

I googled it again and knew it was in libc6-dev（http://download.gna.org/pdbv/demo_html/demo_2.0.10/package/libc6-dev_2.3.2.ds1-20.html）, and I began to find the way how to install libc6-dev. I found this(https://github.com/gliderlabs/docker-alpine/issues/149), andyshinn has done some work in that, but after I installed those libs, the problem could not be resolved.

In that point, I had a idea to know what socketvar.h's content is, so I run as below in my :

    [work@test001 ~]$ cat /usr/include/sys/socketvar.h
    /* This header is used on many systems but for GNU we have everything
       already defined in the standard header.  */
    #include <sys/socket.h>

Oh...so I copied my host machine's socketvar.h into my contianer and built mod_jk again, it succeeded.
