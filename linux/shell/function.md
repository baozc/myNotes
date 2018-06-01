### function

　　什么是『函数 (function)』功能啊？简单的说，其实， 函数可以在 shell script 当中做出一个类似自订执行指令的东西，最大的功能是， 可以简化我们很多的程序代码。

```shell
function fname() {
	#程序段
}
```
调用，直接输入函数名（带不带分号不确定）

　　function 也是拥有内建变量的～他的内建变量与 shell script 很类似， 函数名称代表示 $0 ，而后续接的变量也是以 $1, $2... 来取代的～

例：
```shell
export PATH

function printit(){
echo "Your choice is $1"
}

echo "This program will print your selection !"

case $1 in
	"one")
		printit 1
		;;
	"two")
		printit 2
		;;
	"three")
		printit 3
		;;
	*)
		echo "Usage {one|two|three}"
		;;
esac
```
