```python ln=false
def inputNum(prompt = "Enter a number:"):
	continue_loop = True  
	while continue_loop == True:  
	  x = input(prompt)  
	  try:  
	    x = float(x);  
	    continue_loop = False
	    return x
	  except:  
	    print("Wrong input, please try again.")
```
