# snbookmarklets - bookmarklets for ServiceNow

This is especially useful when you can't use SNUtils or other browser extensions.  Most of these have been taken from ServiceNowDevProgram code-snippets repo.

## Create a new Update Set

When viewing a record in the rm_story table, this bookmarklet will create a update set in a DIFFERENT instance and enable you to pre-populate values. The example below will create an update set in the ficticious "MYDEV" instance and set the Name (`STRY1234 - Short Description`) and Description fields based on values taken from the story record. To use this bookmarklet, udpate the instance name and query string as needed.

```javascript
javascript:
var w=window.frames["gsft_main"]!==undefined?window.frames["gsft_main"]:window;
var q="name="+w.g_form.getValue("number")+" - "+w.g_form.getValue("short_description")+
    "^description=Description:  @"+w.g_form.getValue("description");
top.open("https://MYDEV.service-now.com/sys_update_set.do?sys_id=-1&sysparm_query="+q);
```

## Create a new story task
When viewing a record in the rm_story table, this bookmarklet will create a new child task and enable you to pre-populate values. The example below will create a task of type `Testing` and set the short description to `Test STRY12345 - Short Description` where the story number and short description values are taken from the story record.

```javascript
var w=window.frames["gsft_main"]!==undefined?window.frames["gsft_main"]:window;
var q="parent="+w.g_form.getUniqueValue()+
"^type=4"+
"^short_description=Test "+w.g_form.getValue("number")+" - "+w.g_form.getValue("short_description")+
"^EQ";
top.open("rm_scrum_task.do?sys_id=-1&sysparm_query="+q);
```

## User Impersonation
When a user impersonates another user, the page is redirected to the home page for the new user. This bookmarklet will open the impersonation page in a popup window so the user impersonation can be completed without redirecting the paging being viewed. After selecting the new user, the popup will close and the instance page will refresh with the new user context.
This was updated from a new tab to the popup based on the "Quick Login to current instance" bookmarklet by OrgovanGeza.

```javascript
javascript:let impWin=window.open("/impersonate_dialog.do", "Impersonation", "scrollbars=no,resizable=no,status=no,location=no,toolbar=no,menubar=no,width=300,height=300,left=100,top=100");setInterval(()=>{if(!impWin.location.pathname.includes("impersonate")){impWin.close();window.location.reload();};},500);
```

## Copy URL to ServiceNow Journal

A bookmarklet that can be used on any website to copy the website's title and URL to the clipboard as a [code]...[/code] snippet that can be pasted to a Journal field to create a "fancy" clickable link in Comments or Work notes.

```javascript
javascript:function copyToClipboard(t,e){link_plaintext='[code]<a href="'+e+'" target=_blank>'+t+"</a>[/code]",link_formatted='<a href="'+e+'">'+t+"</a>";var a=function(t){t.preventDefault(),t.clipboardData.setData("text/html",link_formatted),t.clipboardData.setData("text/plain",link_plaintext)};document.addEventListener("copy",a),document.execCommand("copy"),document.removeEventListener("copy",a)}copyToClipboard(document.title,window.location.href);
```

## Open copied record
Someone sent you the Number of a record that belong to the task table (e.g. Incident, SCTASK, RITM, Story, Problem, Change etc.), but not a link to the record itself?

You can save yourself a couple of clicks if you use this Bookmarklet while you are on the instance that the record belongs to.

Usage:

copy the record number,
make sure that in your browser you are on the same instance that the record belongs to,
click the Bookmarklet, and the record's form will open in a new tab (with a visible navigation pane).
I use this Bookmarklet on a daily basis, hope you'll like it as well.

```javascript
javascript: (c=> c.readText().then( text => window.open("http://" + window.location.hostname + "/nav_to.do?uri=task.do?sysparm_query=number=" + text, "_blank")))(navigator.clipboard);
```

## Quick Note

A bookmarklet that opens a new browser tab with a blank editable page for quick notes. Copy and paste work with or without (right-click) formatting. Includes javascript string methods `'variable_name'.get()` to get the contents of the page into a variable and `variable_name.set()` to set the variable contents into another page. This allows for quick processing and output of the results.

```html
data:text/html, <title>Quick Note</title><script>String.prototype.get=function(){window[this]=document.body.innerText};String.prototype.set=function(){w=window.open();w.document.body.innerHTML='<pre style="font: 1rem/1.5 monospace;margin:0 auto;padding:2rem;">'+this.toString()+'</pre>'};</script><body contenteditable style="font: 1rem/1.5 monospace;margin:0 auto;padding:2rem;">
```

### Example
Enter some text onto the page.  
> `This is some text`  

Open the Browser Console.  
Type a string literal with the `.get()` method.   
> `'t'.get()`  

The string literal becomes a variable name containing the contents of the page.  
You can now manipulate the string however needed.  
To see the results, you can simply log the info to the console or you can post it to a new page. To post the content to a new page, call the `.set()` method.  
> `variable_name.set()`

The command below will open a new tab containing "THIS IS SOME TEXT"  
> `t.toUpperCase().set();` 

```javascript
data:text/html, <title>Quick Note</title><script>String.prototype.get=function(){window[this]=document.body.innerText};String.prototype.set=function(){w=window.open();w.document.body.innerHTML='<pre style="font: 1rem/1.5 monospace;margin:0 auto;padding:2rem;">'+this.toString()+'</pre>'};</script><body contenteditable style="font: 1rem/1.5 monospace;margin:0 auto;padding:2rem;">
```

## Quick Login to current instance
If you frequently have to login to the currently used instance with a different user (e.g. with your admin account), this Bookmarklet will be a huge timesaver for you!

All you have to do is to click the Bookmarklet, and a popup window will appear with the current Servicenow instance's login page. After you enter your credentials (or your password manager enters it), the popup will close, and the page will reload to log you in.

```javascript
javascript:
let params = `scrollbars=no,resizable=no,status=no,location=no,toolbar=no,menubar=no,width=300,height=300,left=100,top=100`;
let loginWin = window.open("http://" + window.location.hostname + "/login", "login", params);
setInterval(()=>{
  if(!loginWin.location.pathname.includes("login")){
    loginWin.close();
    window.location.reload();
  };},500);
```
