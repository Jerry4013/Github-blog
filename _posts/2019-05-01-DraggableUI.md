---
layout: post
title:  "Create a Graggable UI with JQuery UI library"
tags: JQuery, Graggable, I
---

## Create a graggable component

I used jquery UI library: `<script src="//code.jquery.com/ui/1.10.4/jquery-ui.js"></script>`

I also used some bootstrap.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Carpool</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
    <script src="//code.jquery.com/jquery-1.9.1.js"></script>
    <script src="//code.jquery.com/ui/1.10.4/jquery-ui.js"></script>
    <script>
        $(function() {
            $( ".draggable" ).draggable();
            $( ".droppable" ).droppable();
        });
    </script>
</head>
<body>
<div class="container" style="margin: auto;float: left;">
    <h2>Carpool</h2>
    <h3 id="team">Team:</h3>

    <h4>Drag your name</h4>
    <h3 id="driver">Drivers:</h3>
    <br />
    <div class="card draggable container" style="width: 15rem; z-index: 0; float: left;">
        <h3>Zeina: 4</h3>
        <div class="card-body ui-widget-content">
            <p class="droppalbe"> + </p>
            <p class="droppalbe"> + </p>
            <p class="droppalbe"> + </p>
            <p class="droppalbe"> + </p>
        </div>
    </div>
    <div class="card draggable" style="width: 15rem; z-index: 0; float: left;">
        <h3>Rischa: 1</h3>
        <div class="card-body dropdown ui-widget-content">
            <p> + </p>
        </div>
    </div>
    <div class="card draggable" style="width: 15rem; z-index: 0; float: left;">
        <h3>Roy: 4</h3>
        <div class="card-body dropdown ui-widget-content">
            <p> + </p>
            <p> + </p>
            <p> + </p>
            <p> + </p>
        </div>
    </div>
</div>

<script>
    var members = ["Radu","Alain","Mohannad","Azzedine","Jerry","Alexandre","Jonathan","Luvleen","Farid",
        "Jennifer","Wanling","Zineb"];
    for (var i = 0; i < members.length; i++) {
        $("#team").after($("<p class=\"card draggable\" " +
            "style=\"color: white; background-color: dodgerblue; width: 8rem;z-index: 1;float: left;\"></p>").text(members[i]));
    }
</script>
</body>
</html>
```

