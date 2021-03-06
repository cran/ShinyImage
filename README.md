
# ShinyImage

Imaging package, with an emphasis on *journaling*, i.e.  recording
history of changes.  Undo/redo operations, ability to display multiple
versions (currently under construction), etc.  The history is
persistent, i.e. across sessions.  Can be run from the R command line,
or from a Shiny-based GUI.

## Installation

You will need the following packages for the command-line interface to the
package:

<UL>

<li> 
<b>fftwtools</b>:  Install from CRAN, except for Linux; for the latter,
  sse 
<a href="#Linux">these special instructions.</a> 
</li> </p> 

<li>
<b>EBImage</b>:  Run these commands from within R:
</p>

```R
source("http://bioconductor.org/biocLite.R", verbose = FALSE) #Install package
biocLite("EBImage", suppressUpdates=TRUE, suppressAutoUpdate=FALSE, ask = FALSE)
```
</li> </p> 

<li>
<b>GUI Installation</b>: Install from CRAN. 

```R
install.packages(c('shiny','shinyjs'))
```
</li> </p> 

</UL>

Having done this, you can install ShinyImage.  For instance, download
the **.zip** package available [here](//github.com/matloff/ShinyImag) and
unpack it, creating a directory/folder **ShinyImage-master**.  Then from a
terminal window, run 

```
R CMD build ShinyImage-master
R CMD INSTALL -l z ShinyImage_0.1.0.tar.gz
```

with __z__ being the location you wish to install ShinyImg to
(changing the version number as necessary).

Alternatively, ShinyImage can be installed using devtools. User's working directory must be set to ShinyImage-master. From R, 
```
> install.packages(c('devtools', 'roxygen2'))
> devtools::install()
```

## Example Usage

Here we will perform several actions, both to illustrate some ShinyImage
operations and also to show the journaling.  All operations will use the
R command line; examples of the GUI are given later in this document.

```R
# load image, whether local file or from the Web
# the image being used is titled 'A tiger in the water'
#  By Bob Jagendorf 
#  [CC BY 2.0 (http://creativecommons.org/licenses/by/2.0)], 
#  via Wikimedia Commons
> tiger <- 
   shinyimg$new("https://upload.wikimedia.org/wikipedia/commons/1/1c/Tigerwater_edit2.jpg")

# 'tiger' is an object of class 'shinyimg', which in turn is a subclass
# of 'R6'

# set autodisplay on 
# after first image modification, 
# image will render and pop up in a new window
# or user can manually render image
> tiger$set_autodisplay()
# manually rendering image
> tiger$render()
# crop the image
> tiger$crop()
[1] "Select the two opposite corners of a rectangle on the plot."
# add brightness
> tiger$add_brightness()
# add contrast
> tiger$add_contrast()
# add gamma
> tiger$add_gamma()
# add blur
> tiger$add_blur()


# remove brightness
> tiger$remove_brightness()
# remove contrast
> tiger$remove_contrast()
# remove gamma
> tiger$remove_gamma()
# remove blur
> tiger$remove_blur()

# we have had nine actions, and can undo the last 8 of them
# we will undo the last five actions (remove blur and remove gamma)
# by calling undo five times
# undoes the removal of the blur
> tiger$undo()
# undoes the removal of gamma 
> tiger$undo()
# undoes the removal of contrast
> tiger$undo()
#undoes the removal of brightness
> tiger$undo()
# undoes the adding of the blur 
> tiger$undo()
# we can also redo the adding of the blur
> tiger$redo()

# we can also save the image to edit later on
> tiger$save("tiger-water.si")
# and later we can come back after a cold boot to do:
> tiger <- shinyload("tiger-water.si")
# if you want to revert to a previous saved state, you can also do:
> tiger$load("tiger-water.si")
# this will load the image back to the state it was in when you saved the image.
> tiger$undo()  # not too late to undo changes made before the save!

# lastly, if we want to save the physical image
> tiger$saveImage('tiger.jpg')
# we can save it as either jpg, png, or tiff
> tiger$saveImage('tiger.png')
# if a user does not specify the name, the default is temp.jpeg
> tiger$saveImage()
```

## GUI Installation and Usage

### Installation

Download from CRAN:

```R
install.packages(c('shiny','shinyjs'))
```

### Usage
<li> A gui can also be spawned to edit images using the sample provided or a user can upload an image, link, or an image log of a .si object created through shinyimg
</p>

<li> A user can edit brightness, contrast, and gamma correction. The user can also rotate, blur, and crop an image. These changes can be made to the image using the sliders. In order to crop a photo, the user has to highlight a box over the original plot. A preview of the cropped image with pop up below the original image. To keep the cropped image, click the keep button which will pop up below the preview image.  
</p>

<li> While editing an image, a user can undo, redo, or reset the image. These actions are executed through buttons at the bottom of the sidebar. 
</p>

<li> After editing an image, a user can download the image and the image log. These actions are below the main plot. 
</p>

<li> The user can also view the image log to see which actions were recorded. 
</p>

Run these commands from within R. 

```R
> runShiny()
```

<li> A user can use an image they are currently editing on the commandline to edit in the GUI
</p>

```R
# using our previous example of our shinyimg object tiger
> runShiny(tiger)
```

<h3>
<a name="Linux">Installing fftwtools on Linux </a> 
</h3>

<UL>

<li> Download http://www.fftw.org/fftw-3.3.6-pl1.tar.gz
</li> </p> 

<li> Unpack, say to x/fftw-3.3.6-pl1 and from that directory run
</p>

<pre>
./configure --prefix=y --enable-shared=yes 
</pre>

<p>
where <b>y</b> is your desired installation directory for <b>fftwtools</b>,
say <b>/usr/local</b>.
</li> </p>

<li> Run the usual <b>make; make install</b> sequence.
</li> </p>

<li> Set environment variables (no spaces around the = sign!):
</p>

<pre>
export C_INCLUDE_PATH=x/fftw-3.3.6-pl1/api 
export LD_RUN_PATH=y/lib 
export LIBRARY_PATH=y/lib 
</pre>
</li> </p>

<li> You may need to install <b>libtiff-dev</b>, say by 
</p>

<pre>
sudo apt-get install libtiff-dev
</pre>
</li> </p> 

<li> You may also need to install <b>fftw-dev</b>, by 
</p>

<pre>
sudo apt-get install fftw-dev
</pre>
</li> </p> 

<li> If fftw library does not get properly installed, try
</p>

<pre>
sudo apt-get install fftw3 fftw3-dev pkg-config
</pre>
</li> </p> 



<li> Then run the R steps as above.
</li> </p>

</UL>
