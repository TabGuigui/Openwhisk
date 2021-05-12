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
