# ExamNotes2017
*[Disclaimer: Contents of these notes are teaching materials that belong to the [School of Computer Science, University of Birmingham](www.cs.bham.ac.uk)]*

Notes for UoB CS exam 2017 made purely with HTML/Javascript/CSS.
Notes can be found [here](https://rinaldyr.github.io/ExamNotes2017 "https://rinaldyr.github.io/ExamNotes2017"). 

https://rinaldyr.github.io/ExamNotes2017

Making notes while learning these languages feels like "killing two birds with one stone".

Anyone who finds this is welcome to use it or help build it up before exam starts.
Hopefully these notes will remain useful for the future generations too! 

## Notes Templates Guide
No need to follow, but it might be useful if you wanna do some notes making in html (for this project). Most of these already have CSS bound to them, following Rhys' template. 

####HTML
######Template
`<h1> Module Title </h1>` &ndash; For module title

`<h2> 1 &ndash; Section </h2>` &ndash; For section 1 title

`<h3> 1.1 &ndash; Subsection </h3>` &ndash; For subsection 1.1 title 

`<h4> Subsubsection </h4>` &ndash; For subsubsection title

`<hr/>` &ndash; A horizontal line to indicate section finished. 
 
 `<!-- This is a comment -->` &ndash; A line of comment in HTML. 
 
 ######Keywords
`<keyword> primary keyword </keyword>` &ndash; To emphasise a **primary keyword**. This is a custom tag so it will only work for this project. 

`<strong> secondary keyword </strong>` &ndash; For <strong> secondary keyword </strong>

######Linking
`<a href="www.google.com"> Link </a>` &ndash; To anchor the surrounded text with a link. Go to that link when clicked. Example above <a href="www.google.com">Link</a>.

* `<anyTag id="uniqueID"></anyTag>` &ndash; To assign an ID on this tag. This will be useful for table of content links. Usually specified at every *Section* and *Subsection*.
* `<a href="#id"> Section </a>` &ndash; The hashtag <code>#</code> will link to the specified ID element in the page.

######Writing Code
`<code> Line of code </code>` &ndash; For any line of code. Makes the font <code>monospaced</code>. 
```html
<pre>
    public class HelloWorld {
        public static void main(String[] args) {
            System.out.println("Hello world");
        }
    }
</pre>
```

For any block of code. This also recognises tabs and white spaces. So just type code inside this tag as usual. Output of above: 
<pre>
    public class HelloWorld {
        public static void main(String[] args) {
            System.out.println("Hello world");
        }
    }
</pre>

######Updating Progress Bar
Look for the module row in index.html, then seek for this block of code inside that row, and change the <code>value=" "</code> parameter.
```html
<tr> <!-- Module Title --> 
    ... 
    <td> 
      <!-- This is the progress bar -->
        <div class="parentBar"> 
            <div class="childBar" value="5" max="11"></div>
        </div>
      <!-- End of progress bar -->
   </td> 
</tr>
```


## WebStorm
It is recommended to use [WebStorm](https://www.jetbrains.com/webstorm/download/) from JetBrains. 
It is a powerful IDE which will spot most of your mistakes and allows version control directly. Also comes with a **live** markdown(.md)
editor.

Some useful default WebStorm shortcuts:

*Hover* around the **top right corner** of the editor &ndash; it should reveal some browsers to select from. These are fast ways to preview your stuff. It is not a 'live' preview, however.

`Shift + Enter` &ndash; Enter a *new line* in the editor and move caret to the start of that new line. Saves some time from needing to press arrow keys.  

`[Highlight Text] Alt + Shift + Z then T` &ndash; This command allows the highlighted text to be *surrounded with a tag*, then type the tag name. `<>text</>`

`[Highlight Text] Ctrl + Shift + /` &ndash; Instantly *comments* a block of highlighted text.

## Who are we?
All currently second year students in the University of Birmingham taking Computer Science. Alphabetically: 

* Ahmed Bhallo 
* Harvey Hyatt
* Karanvir Kalsi
* Sivarjuen Ravichandran
* Rinaldy Ridwan
* Rhys Barrett. 