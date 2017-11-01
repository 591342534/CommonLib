# CommonLib
##How to use?
线程安全的服务器开发基础类库
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <sys/un.h>
#include <sys/time.h>
#include <netinet/in.h>
#include <netinet/tcp.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>
#include <netdb.h>
#include <errno.h>
#include <stdarg.h>
#include <stdio.h>

static int uc_set_tcpnodelay(int fd, int val)
{
    if (setsockopt(fd, IPPROTO_TCP, TCP_NODELAY, &val, sizeof(val)) == -1)
    {
    	return -1;
    }
    return 0;
}
static int uc_setblock(int fd, int non_block)
{
    int flags;

    if ((flags = fcntl(fd, F_GETFL)) == -1) {
        return -1;
    }

    if (non_block)
        flags |= O_NONBLOCK;
    else
        flags &= ~O_NONBLOCK;

    if (fcntl(fd, F_SETFL, flags) == -1) {
       return -1;
    }
    return 0;
}

//设置非阻塞，成功返回0，失败返回-1
int uc_nonblock(int fd) 
{
    return uc_setblock(fd,1);
}
//设置阻塞，成功返回0，失败返回-1
int uc_block(int fd) {
    return uc_setblock(fd,0);
}
