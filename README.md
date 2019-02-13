# shadow-dom-automation
This project focuses on automation of shadow root dom using javascript. You can use selenium or any javascript executer to execute it. You can embed this with your current automation framework with few changes.

## Shadow DOM:
Shadow DOM is a web standard that offers component style and markup encapsulation. It is a critically important piece of the Web Components story as it ensures that a component will work in any environment even if other CSS or JavaScript is at play on the page.

## Problem Statement:
You have already developed your web-based automation framework in java selenium or ruby watir or any other tool.Your frontend application  was developed in some frontend technology and you was using your automation framework provided methods to execute your command and to perform operation on web elemets. Later on developer changes their strategy to develop frontend to use Polymer that uses shadow dom. 
Now due to this small change in strategy of development, you are stuck because your framework or tool provided methods are not able to identify elements that resides under a shadow-root dom element. 

## Solution:
You don't want to loose your already commited automation testcases and framework & you want the easiest approach that can be embedded with your current autaomation tool. I am here to show you the right path. You have one and only one choice and that is JavaScript :) . Yes, you heard it right. Javascript can be used to navigate in shadow-root dom elements.
This seems really easy haha, but its not that easy my friend. Its really a cumbersome process to use javascript in your current automation framework, and it also increases testcases development & mentainance time as well as it makes really difficult to debug if anything went wrong. 
Due to this I tried to simplify the process by creating some reusable javascript methods.

## How it works:
You will embed this javascript prgram to your page where you have shadow-root elements. And thats it my friend , this will let you execute all the methods of this javascripty program that will access you current page DOM structure and it will return you the DOM element as Object. Now you can use this object directly (if not then you will have to change it to your framework element, most of the time it works without casting and all) to call your framework methods like .click() .set() etc.

## Methods:
  getObject(selector, root = document) : use this method if want single element from DOM

  getAllObject(selector, root = document) : use this if you want to find nth element from DOM

You can use this iteratively by changing root = <your current dom>
  
###### How to embed querySelector javascript file:
  You will have to create a custoom method somewhere that will call your framework provided javascript execute method.
  
  ```ruby
  def execute_script(script, *args)
		add_automation_js = <<-JS
			var script_present=false;
			var all_items = Array.from(document.getElementsByTagName("body")[0].getElementsByTagName("script"));
			all_items.forEach(function(element) {
				  let name = element.src;
				  if(name.include("querySelectorDeep.js")) {
					script_present=true;
				  }
			});
			if(!script_present) {
				var bootscript = document.createElement("script");
				bootscript.setAttribute("type", "text/javascript");
				bootscript.setAttribute("language", "javascript");
				bootscript.setAttribute("src", "//localhost:8082/a/querySelectorDeep.js");
				document.getElementsByTagName("body")[0].appendChild(bootscript);
			}
		JS
		$browser.execute_script(add_automation_js)
		sleep 2
		return $browser.execute_script(script, *args)
	end
  ```
## Selector:
  ###### Examples: 
  for html tag ``` <paper-tab title="Settings"> ```
  You can use this code in your framework to grab the paper-tab element Object.
  ```ruby
    Lib.execute_script("return getObject('paper-tab[title=\"Settings\"]');")
  ```
  for html tag that resides under a shadow-root dom element ``` <input title="The name of the employee"> ```
  You can use this code in your framework to grab the paper-tab element Object.
  ```ruby
    Lib.execute_script("return getObject('input[title=\"The name of the employee\"]');")
  ```
  for html tag that resides under a shadow-root dom element 
  ``` 
  <properties-page id="settingsPage"> 
    <textarea id="textarea">
  </properties-page>
  ```
  You can use this code in your framework to grab the textarea element Object.
  ```ruby
    Lib.execute_script("return getObject('properties-page#settingsPage>textarea#textarea');")
  ```
  
  ###### Note: > is used to combine multi level dom structure. So you can combine 3 levels of dom. If you want some more level modify the script and ready to rock.
  
