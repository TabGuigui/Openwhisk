# how to create and invoke a easy action

1. make a python file which contains "main" function

```
def main(args):
    name =  args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    place = args.get("place", "somewhere")

    print(greeting)
    return {"greeting": greeting,"place":place}
```
**the input and output should be a dict**

2. create an action

```
wsk action -i create helloPython hello.py # helloPython is action name, hello.py is the function including main  
wsk action -i invoke --result helloPython --param name Tab # --result param show the result directly
{
    "greeting": "Hello Tab!"
}
```

3. param
```
$ more param ：
		
wsk action -i invoke --result helloPython --param name Tab --param place China

# default param ：
wsk action -i update helloPyhon --param place China #  when incoke, still can use a param to overwrite the default param 
```


4. trigger

触发器可以通过使用键值对字典来触发（激活）。有时这本词典被称为事件。与操作一样，每次触发触发器都会产生一个激活ID。 触发器可以由用户显式激发，也可以由外部事件源代表用户激发。 rule相当于把一个trigger 和 action联系起来， 这样当激活一个trigger的时候， 相应的action就会被invoke起来。


构建流程：

构建一个trigger 
	
	wsk trigger update locationUpdate

将trigger与action通过一个rule连接起来

	wsk rule create myRule locationUpdate helloJS

激活trigger

	wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C." 
	ok: triggered /_/locationUpdate with id cef0150de3744ac6b0150de3743ac6fd

	./wsk activation -i result 7313329c5090442993329c50905429ec
	{
	    "payload": "Hello, Donaldfrom Washington, D.C."
	}

trigger fire 的参数 ：  
-p 键 值
-P 一个json文件。
