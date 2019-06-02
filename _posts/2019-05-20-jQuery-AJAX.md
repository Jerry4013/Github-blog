---
title:  "jQuery AJAX"
tags: jQuery, AJAX
---
### \#Tip of the Day: Finished my first e-Commerce website today! Very happy!

<br /><br />

## jQuery load() method

The load() method loads data from a server and puts the returned data into the selected element.

Syntax:

```javascript
$(selector).load(URL, data, callback);
```

The callback function can have differenct parameters:

* responseTxt - contains the resulting content if the call succeeds
* statusTxt - contains the status of the call
* xhr - contains the XMLHttpRequest object

```javascript
$("button").click(function(){
  $("#div1").load("demo_test.txt", function(responseTxt, statusTxt, xhr){
    if(statusTxt == "success")
      alert("External content loaded successfully!");
    if(statusTxt == "error")
      alert("Error: " + xhr.status + ": " + xhr.statusText);
  });
});
```

## jQuery $.get() method

```javascript
$.get(URL,callback);
```

Example:
```javascript
$("button").click(function(){
    $.get("demo_test.asp", function(data,status){
        alert("Data: " + data + "\nStatus: " + status);
    });
});
```

## jQuery $.post() method

Syntax:
```javascript
$.post(URL,data,callback);
```

Example:
```javascript
$("button").click(function(){
  $.post("demo_test_post.asp",
  {
    name: "Donald Duck",
    city: "Duckburg"
  },
  function(data, status){
    alert("Data: " + data + "\nStatus: " + status);
  });
});
```
## jQuery $.ajax() method

```javascript
$(document).ready(function(){
    $("#search").click(function() {
        $.ajax({
            type: "GET",
            url: "service.php?number=" + $("#keyword").val(),
            dataType:"json",
            success: function(data) {
              if (data.success){
                  $("#searchResult").html(data.msg);
              } else {
                  $("#searchResult").html("error");
              }
            },
            error: function(xhr) {
              alert("error" + xhr.status);
            }
        }); 
    });
});
```