# 程序说明
本程序使用python3编写，作者黄晓龙 \
程序包含一个类 MazeFactory 和 主程序部分 \
## 在类 MazeFactory 中：
1. 
```
def __init__(self, command_line1, command_line2):
```
__init__方法使用输入的两行字符串进行初始化对象
2. 

```
def Render(self):
```
Render函数返回该迷宫对象渲染后的字符串

## 在主程序中：
1. 

```
command_line1 = input()
command_line2 = input()
```
键盘输入两行字符串
2. 

```
maze = MazeFactory(command_line1, command_line2)
```
创建迷宫对象maze并利用输入来初始化该对象
3. 

```
mazeText = maze.Render()
print(mazeText)
```
用maze对象的Render()方法返回渲染后的字符串并输出
# 程序运行及测试
在命令行中进入到程序所在目录maze_factory \
运行以下命令

```
chmod a+x maze.py
```
将maze.py转化为可执行文件 \
然后输入

```
./maze.py
```
程序运行开始，如图
![屏幕快照 2018-07-28 下午6.55.09.png](http://note.youdao.com/yws/res/185/WEBRESOURCE5bcc3cf9348a0f422630c38c81f09107)
## 测试用例验证
输入题目中的测试用例输入：

```
 3 3
 0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
```
得到输出：

```
[W] [W] [W] [W] [W] [W] [W]
[W] [R] [W] [R] [R] [R] [W]
[W] [R] [W] [R] [W] [R] [W]
[W] [R] [R] [R] [R] [R] [W]
[W] [W] [W] [R] [W] [R] [W]
[W] [R] [R] [R] [W] [R] [W]
[W] [W] [W] [W] [W] [W] [W]
```
如下图：
![屏幕快照 2018-07-28 下午7.00.37.png](http://note.youdao.com/yws/res/194/WEBRESOURCE70d9739212d16cfc05ac68ad44b60752)
得到的输出与题中测试用例的输出==完全一致==

## 输入数据有效性测试
1. 无效的数字
![屏幕快照 2018-07-28 下午7.06.35.png](http://note.youdao.com/yws/res/204/WEBRESOURCE49321209c28701048bb596befe537a4f)
将测试用例输入中第二行第一个数字1改为字母a \
程序输出为字符串 ”Invalid number format.”
2. 数字超出预定范围
![屏幕快照 2018-07-28 下午7.15.04.png](http://note.youdao.com/yws/res/226/WEBRESOURCE04e8a85dd66d4e5a95cd5b27c23bcfcb)
![屏幕快照 2018-07-28 下午7.15.59.png](http://note.youdao.com/yws/res/229/WEBRESOURCE61a1657acc60238ceae7f4fb6c42690a)\
输入中有负数或超过网格范围 \
程序输出为字符串 ”Number out of range.”
3. 格式错误
![屏幕快照 2018-07-28 下午7.18.56.png](http://note.youdao.com/yws/res/236/WEBRESOURCEa57dc65ae4128a10ac61bec07dd184e6)\
输入数据格式错误 \
程序输出为字符串 ”Incorrect command format.”
4. 连通性错误
![屏幕快照 2018-07-28 下午7.23.50.png](http://note.youdao.com/yws/res/245/WEBRESOURCE2613a1724c19dc5ee9e53897de649bf5)\
输入的两个网格无法连通 \
程序输出为字符串 ”Maze format error.”

# 代码及注释

```
#!/usr/bin/env python3
# -*- coding: UTF-8 -*-

class MazeFactory:

    def __init__(self, command_line1, command_line2):

        # answer_tag 用来作为输入有效的标志。
        # answer_tag 值默认为0代表输入有效，不为0代表输入无效。
        # answer_tag 值为1代表无效的数字:输入的字符串无法正确的转换为数字；
        # answer_tag 值为2代表数字超出预定范围:数字超出了允许的范围，例如为负数等；
        # answer_tag 值为3代表格式错误:输入命令的格式不符合约定；
        # answer_tag 值为4代表连通性错误:如果两个网格无法连通，则属于这种错误。；
        self.answer_tag = 0

        if '*' in command_line1:
            s = command_line1.split('*')
        else:
            s = command_line1.split(' ')
        l = len(s)
        if (l!=2):
            # 第一行输入格式错误，answer_tag 值置为3
            self.answer_tag = 3
        else:
            isnum_tag = 1

            # maze_row, maze_column为道路迷宫尺寸
            maze_row = 0
            maze_column = 0

            # 尝试将输入的字符串转为数字
            try:
                maze_row = int(s[0])
                maze_column = int(s[1])
            except:
                isnum_tag = 0

            if (isnum_tag == 0):
                # 第一行输入的字符串无法正确的转换为数字，answer_tag 值置为1
                self.answer_tag = 1

            elif ((maze_row <= 0)or(maze_column <= 0)):
                # 第一行输入的数字超出预定范围，answer_tag 值置为2
                self.answer_tag = 2

            else:
                # self.row, self.column为渲染网格尺寸
                self.row = maze_row * 2 + 1
                self.column = maze_column * 2 + 1

                # 将渲染网格matrix初始化，道路值为1，墙壁值为0
                self.matrix = [[0 for i in range(self.column)] for i in range(self.row)]
                for i in range(maze_row):
                    for j in range(maze_column):
                        self.matrix[i * 2 + 1][j * 2 + 1] = 1

                # 判断连通性输入的有效性，若有效则根据输入打通道路
                s = command_line2.split(';')
                for x in s:
                    path = x.split(' ')
                    if (len(path)!=2):
                        # 第二行输入格式错误，answer_tag 值置为3
                        self.answer_tag = 3

                    else:
                        pos1 = path[0].split(',')
                        pos2 = path[1].split(',')
                        if ((len(pos1) != 2) or (len(pos1) != 2)):
                            # 第二行输入格式错误，answer_tag 值置为3
                            self.answer_tag = 3
                        else:
                            isnum_tag = 1

                            # 尝试将输入的字符串转为数字
                            try:
                                x1 = int(pos1[0])
                                y1 = int(pos1[1])
                                x2 = int(pos2[0])
                                y2 = int(pos2[1])
                            except:
                                isnum_tag = 0

                            if (isnum_tag == 0):
                                # 第二行输入的字符串无法正确的转换为数字，answer_tag 值置为1
                                self.answer_tag = 1

                            elif ((x1<0) or (x1>=maze_row) or (y1<0) or (y1>=maze_column)
                                or (x2<0) or (x2>=maze_row) or (y2<0) or (y2>=maze_column)):
                                # 第二行输入的数字超出预定范围，answer_tag 值置为2
                                self.answer_tag = 2

                            # 若两个网格无法连通，answer_tag 值置为4；否则将渲染网格对应cell之间的道路打通，即值置为1
                            elif (x1 == x2):
                                if (abs(y1 - y2) != 1):
                                    self.answer_tag = 4
                                else:
                                    maze_x = x1 * 2 + 1
                                    maze_y = y1 + y2 + 1
                                    self.matrix[maze_x][maze_y] = 1
                            elif (y1 == y2):
                                if (abs(x1 - x2) != 1):
                                    self.answer_tag = 4
                                else:
                                    maze_y = y1 * 2 + 1
                                    maze_x = x1 + x2 + 1
                                    self.matrix[maze_x][maze_y] = 1
                            else:
                                self.answer_tag = 4


    def Render(self):
        ans_str = ''
        if (self.answer_tag == 1):
            ans_str = 'Invalid number format​.'
        elif (self.answer_tag == 2):
            ans_str = 'Number out of range​.'
        elif (self.answer_tag == 3):
            ans_str = 'Incorrect command format​.​'
        elif (self.answer_tag == 4):
            ans_str = 'Maze format error.​'
        else:
            for i in range(self.row):
                for j in range(self.column):
                    sub = ''
                    if (self.matrix[i][j] == 0):
                        sub = '[W]'
                    if (self.matrix[i][j] == 1):
                        sub = '[R]'
                    if (j != self.column - 1):
                        sub = sub + ' '
                    elif (i != self.row - 1):
                        sub = sub + '\n'
                    ans_str = ans_str + sub
        return ans_str

if __name__ == "__main__":

    #输入命令字符串
    command_line1 = input()
    command_line2 = input()

    #利用输入命令初始化迷宫，初始化过程中检查输入的有效性
    maze = MazeFactory(command_line1, command_line2)

    #将迷宫渲染为字符串
    mazeText = maze.Render()

    #输出渲染后的字符串
    print(mazeText)

```
