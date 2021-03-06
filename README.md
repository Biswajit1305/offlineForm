offlineForm
===========

Stores the forms in the localStorage when the browser is offline and synchronizes them when the browser is back online

### jsFiddle Demo

http://jsfiddle.net/Brewal/ZZwdD/

### How to use it
1. Include the plugin in your head tag :  

    `````html
    <script type="text/javascript" src="path/to/offlineform.jquery.js"></script>
    `````
    Or the minified version : 
    `````html
    <script type="text/javascript" src="path/to/offlineform.jquery.min.js"></script>
    `````
    
2. Initialize the plugin. Here is a full initialization with all default options and events handler :  

    `````javascript
    $(document).ready(function(){
        $('form.offline').offlineForm({
            key: "offlineForm",
            classname: "offlineForm",
            onlineAjaxSend: false,
            autoSync: true,
            beforeSync: null,
            afterSync: null,
            onSync: null,
            beforeStorage: null,
            onStorage: null,
            onError: null,
            beforeOnlineAjaxSend: null,
            onlineAjaxCallback: null
        });
    });
    `````
    **Options**  
    * `key` : localStorage key name of the plugin.  
        * default : `offlineForm`  
        * type : `String`
    * *classname* : CSS classname to add to the form.  
        * default : `offlineForm`  
        * type : `String`  
    * *onlineAjaxSend* : wether to send the form with ajax when online or not.
        * default : `false`
        * type : `Boolean`
    * *autoSync* : if true, send automatically the forms when an online event is detected
        * default : `true`
        * type : `Boolean`
    
    **Events** (all events are "null" by default)
    * *beforeSync* : called before syncing all local data.  
        arguments :  
        * `total` : number of forms to synchronize  
    * *afterSync* : called when forms are all synchronized.  
        arguments :  
        * `total` : number of forms synchronized  
    * *onSync* : called when a single form is syncrhonized.  
        arguments :  
        * `progress` : number of forms already sent  
        * `total` : number of forms to synchronize  
        * `response` : the response of the server 
    * *beforeStorage* : called before storing a form in the localStorage
        arguments : 
        * `total` : number of forms to synchronize once back online
        * `data` : localStorage data
    * *onStorage* : called when offline and when a form is submited  
        arguments :  
        * `total` : number of forms to synchronize once back online
        * `data` : localStorage data
    * *onError* : called when an error occured during syncing (404, 500, ...)  
    * *beforeOnlineAjaxSend* : called before sending the ajax request when online
    * *onlineAjaxCallback* : callback for online ajax sending (useful when onlineAjaxSend is set to true)  
        arguments :  
        * `response` : the response of the server  

3. If you choose to define `autoSync` to `false`, you can trigger a sync by yourself : 

    `````javascript
    eventOnline = function(){
        $('#myForm').offlineForm('sync');
    };
    if(window.addEventListener) {
        window.addEventListener('online', eventOnline);
    } else {
        document.body.attachEvent('ononline', eventOnline);
    }
    `````

### Quick example

**HTML**

`````html
<form id="myForm" action="/some/path" method="post">
    <label>Field1</label>
    <input type="text" name="field1" />
    
    <input type="submit" value="Submit the form" />
</form>
`````

**Javascript**

`````javascript
$(document).ready(function(){
    $('#myForm').offlineForm({
        onStorage: function(total) {
            alert('You have send a form but you are offline. It will be submited when back online.');
            alert(total+' form(s) have been locally stored'); 
        }
        afterSync: function(total) {
            alert('All form have been submited. Total : '+total);
        }
    });
});
`````

### Requirements
* jQuery 1.9+
* A web server (such as apache)
* An offline capable browser and/or application

### Credits
* Author : Brewal RENAULT
* Company : [Oziolab](http://oziolab.fr/)