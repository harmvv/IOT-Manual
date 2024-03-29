# IOT-Manual
## Manual for the Youtube Data API V3

### How to implement the YouTube API V3 in your project, get data from the youtube API and display it as Json in the Console.
In this tutorial we are going to generate a key for the YouTube API and create a website where we can use the API to display YouTube content. After this tutorial you know how to get data from YouTube. 

#### Example
An example I made with the Youtube API.
https://herman-janssen-age-gues.herokuapp.com/

#### Disclaimer
After this Tutorial you know how to get data from the API, display this data in the Console and help you get started how to use that data, but this tutorial wil not fully explain how to turn that data to a youtube video on your website. Follow up links will help you, I placed them at the end of this tutorial. 

#### Requirements
1. For this project you will need a text editor.
https://en.wikipedia.org/wiki/Text_editor Read this if you don't know what a text editor is.
https://www.keycdn.com/blog/best-text-editors for the best free text editors

2. Some basic knowledge about Html and JavaScript is handy but not necessary.


### First things First
Go to https://developers.google.com/youtube/v3 and login with your Google account.
If you don’t have a account, create one. Here you can find all the information about the YouTube V3 API and what to do with it.

### New project in the Google Console
1. We have to create a new project in the google console, this is a place where you can find your google projects.
https://console.developers.google.com/?hl=nl

Create a new project in the google developers console. You can do this in the upper left next to the Google API’s logo 

![Image that shows how to create a google project](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Afbeelding1.png)

2. Start a new project, give it a name and follow the steps to generate a key for the YouTube API V3. In the location field you don’t have to change anything but you can create folders.

3. Once created, browse to your project it's in the spot next to the Google API logo.  

 ![Image that shows how to add a new api](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2012.56.40.png)

4. Choose enable API’s and services, search for YouTube and choose the YouTube Data API V3.
 
 ![Image that shows the search for the youtube api](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2012.57.47.png)
 
5. Choose enable and wait a few seconds. The API is now added to your project. 

6. Click in the left menu for Credentials. Normally there should be your YouTube API key but you first need to configure some settings. Choose configure the permission screen. You only have to enter a name there. If you are going to publish the website later you should add more details. Now you can choose for create credentials. 

 ![Image that shows menu](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2014.02.42.png)

7. Choose API key and there you go, your YouTube Data API V3 key.

8. Save this key and protect it, you should treat your key like your password.

### Setting up the project on your PC

1. Create a new folder on your computer for your project. Create a new file: a index.html and a new subfolder called javascripts place a new file inside that folder called script.js.

 ![Image that shows folder structure](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-16%20om%2007.33.36.png)

2. Open the project folder in your text editor and add this to the html for a standard html layout.

```bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script src="javascripts/script.js"></script>

</head>
<body>
    
</body>
</html>
```
We only use this to access the website in the browser. The script tags makes a connetion with your javascript and a Jquery Libary online.

3. In the html we are going to place a search bar, the search bar lives inside a form tag. We are going to use the input we can place there to search YouTube. Place the next form inside the body tags.
```bash
 <form id="search-form" name="search-form" onsubmit="return search()">
    <div class="fieldcontainer">
        <input type="search" name="search-form" id="query" class="search-field" placeholder="search Youtube" >
        <input type="submit" name="search-btn" id="search-btn" value="">

    </div>
    </form>
```
4. Next we are going to start to work in the JavaScript file. Here we are going to make a connection with the project and the YouTube API. We do this with a JavaScript function, and a little use of jQuery. No worry’s you don’t need any knowledge about jQuery. 

5. First we create a little function that gets the input from the search field and when you hit submit it gets the data so we can use in in a other function.

```bash
$(function(){
    var searchField = $("#query");
   

        $("#search-form").submit(function(e){
            e.preventDefault();
        });
})

```
In the next function we are going to declare what kind of information we want to get from the API. In our case this would be : Snippet, ID,Q and Type. (Q is a parameter that specifies the query term to search for)

In the first line of the next code you see we declare the search bar from the HTMl in a variable called q. Under that line of code we make a get request. There we declare a couple of things mentioned above. Replace the key value with your YouTube API key. In this get request we send the input from the search field to the YouTube API and we tell the YouTube API we want the following things back from it.

```bash
function search(){

    // gef form inputs
    q = $('#query').val();

    // run get request on api
    $.get (
        "https://www.googleapis.com/youtube/v3/search",{
            part: 'snippet, id', 
            q : q,
            type : "video",
            key : 'Your key***'},
            function(data){

                    // log data
                    console.log(data);
            }
    );
}
```




Your code should now look something like this





```bash
$(function(){
    var searchField = $("#query");
   

        $("#search-form").submit(function(e){
            e.preventDefault();
        });
})

function search(){

    // gef form inputs
    q = $('#query').val();

    // run get request on api
    $.get (
        "https://www.googleapis.com/youtube/v3/search",{
            part: 'snippet, id', 
            q : q,
            type : "video",
            key : 'Your KEY****'},
            function(data){

                    // log data
                    console.log(data);
            }
    );
}
```


And your HTML like this

```bash
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script src="javascripts/script.js"></script>

</head>
<body>
    <form id="search-form" name="search-form" onsubmit="return search()">
        <div class="fieldcontainer">
            <input type="search" name="search-form" id="query" class="search-field" placeholder="search Youtube" >
            <input type="submit" name="search-btn" id="search-btn" value="">
    
        </div>
        </form>
    
</body>
```

### Time to test our code.

#### The best way to test your code is in Google chrome. If you don’t have this browser download it here. 

https://www.google.com/chrome/?brand=CHBD&gclid=Cj0KCQjw3JXtBRC8ARIsAEBHg4nlfkx3Qm4JkfwwXzHH1LntpUjoV2xHYE713h3NPkgYyaAtx7vqQLAaAmzQEALw_wcB&gclsrc=aw.ds

1. Browse to your project folder, open it, and right click index.html choose open with google chrome 

![Image that shows how to open the index.html in google chrome](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2012.29.30.png)

2. You should see an empty page with a little search bar in the upper left corner. No worries this is great.

Right click somewhere on the page and choose inspect. 

![Image that shows how to open the console in google chrome](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2012.31.38.png)
 
3. You see a popup menu. Choose in the menu for console. If there are any problems with your script, you should see them here. If you don’t see any problems, Great job.

4. Try to type something in the search bar, if you did all the steps correct you should see a text in your console. This text is in Json format. It’s a way of display data that your computer understands. 

![Image that shows the json file from the youtube api](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2012.35.51.png)
 
5. Click on the little arrow on the left and do that again on the arrow before items.
This is an array, an array is nothing more then a couple of objects. This are the 5 first YouTube video’s that will show up if you search YouTube with your search input.  

![Image that shows the json file from the youtube api in details](https://oege.ie.hva.nl/~versevh/afbeeldingen/manual.iot/Schermafbeelding%202019-10-15%20om%2014.06.02.png)


### End result
Maybe this all seems a little boring but it is really cool, you just have printed some information from the YouTube system to your website. And with this information we can display those videos on our own website. It is standard that Youtube prints out the first 5 vidoes if you want more read the documantation.  

If you are interested in showing this information on your website with the video. Then you have to create a code that pushes for every object some html to your page, with a YouTube iframe, you can then display the right video. 

#### Example
Write a code that pushes for every object an iframe like this. 
 ``` bash
      <div class="iframevideo">
         <iframe width="560" height="315" style="display:block;margin:auto;" src="https://www.youtube.com/embed/${Name of the variable where you place the ID of hte youtube video}" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
         </div>
```
For more information about this: 

https://herman-janssen-age-gues.herokuapp.com/ - A project I made with the YouTube Data API V3

https://developers.google.com/youtube/v3/getting-started - YouTube Data API V3

https://developers.google.com/youtube/iframe_api_reference - YouTube iframe information

https://api.jquery.com/append/ - How to push html from a javascript file with Jquery 


