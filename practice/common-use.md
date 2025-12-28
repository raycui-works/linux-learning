## 一：保存代码文件
### Step 1：进入容器并进入练习目录
```bash
cd ~                          # 回到当前用户的家目录（root 的话就是 /root）
mkdir -p c_practice           # 创建一个放 C 代码的目录（已存在就不报错）
cd c_practice                 # 进入该目录
```

### Step 2：把代码写进 fork_demo.c
```bash
cat > fork_demo.c <<'EOF'     # 把下面内容原样写入 fork_demo.c，直到遇到单独一行 EOF 结束
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <err.h>

static void child(void)
{
    printf("I'm child! my pid is %d.\n", getpid());
    exit(EXIT_SUCCESS);
}

static void parent(pid_t pid_c)
{
    printf("I'm parent! my pid is %d and the pid of my child is %d.\n",
           getpid(), pid_c);
    exit(EXIT_SUCCESS);
}

int main(void)
{
    pid_t ret;
    ret = fork();
    if (ret == -1)
        err(EXIT_FAILURE, "fork() failed");

    if (ret == 0) {
        child();
    } else {
        parent(ret);
    }

    err(EXIT_FAILURE, "shouldn't reach here");
}
EOF
```
### Step 3：确认文件保存成功
```bash
ls -l fork_demo.c             # 查看文件是否存在、大小是否合理
sed -n '1,40p' fork_demo.c     # 打印前 40 行，确认内容写对了
```

## 二：编译代码文件
```bash
cc -o fork fork.c
```

## 三：执行代码文件
```bash
./fork
```
