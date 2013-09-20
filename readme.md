## sass-color-guide

Given a bunch of Sass colors, generates a visual representation.

This representation can then be used in style guides.

Ultimately, this tool will come in handy to auto generate part of a style guide based on colors defined in a `_colors.scss` file.

Looks like this with the right CSS:

<img src="http://f.cl.ly/items/200K1c1P3U063D0g0a0s/Screen%20Shot%202013-09-20%20at%2017.11.17.png">



## Sample input

### Colors

	/* ==========================================================================
	  Color variables
	   ========================================================================== */
	
	// Background and text colors (neutral)
	$very-light-color:             #fff     !default;
	$very-subtle-color:            #f5f5f5  !default;
	$subtle-color:                 #DDD     !default;
	$light-medium-color:           #D7C9D6  !default;
	$light-color:                  lighten($light-medium-color, 10%)  !default;
	$muted-color:                  #998197 !default;
	
	$medium-color:                 #3e3744 !default; // rather dark
	$dark-color:                   #3F2B3F !default;
	$very-dark-color:              #000 !default;
	
	// Divider colors
	
	$divider-color:                $light-medium-color !default;
	
	// Colors with meaning e.g. red for error
	
	$success-color:                 #5C9F45;
	$success-color-alt:             #8fae1f;
	$error-color:                   #DB4614;
	$help-color:                    #643ABF;
	$highlight-color:               #F9F3AC;


### Output HTML

Generate this:

	<div id="component-colors">
		<div class="colors">
		
		    <!-- Loop for every color if the color is white add a box shadow -->
		
		    <div>
		        <div class="color-example" style="background: #FFF; box-shadow: 0 0 0 1px #DDD"></div>
		        <p>$very-light-color: #FFF</p>
		    </div>
		    
		    <!-- Else dump color value and add color name -->
		
		    <div>
		        <div class="color-example" style="background: #f5f6f7;"></div>
		        <p>$light-color: #f5f6f7</p>
		    </div>
		    
		</div>
	</div>


### SCSS for output HTML
	
	/* Component: colors 
	   ========================================================================== */
	
	#component-colors {
	  .colors {
	    @include clearfix;
	  }
	  .colors > div {
	    width: 150px;
	    font-size: 11px;
	    float: left;
	    text-align: center;
	  }
	  .color-example {
	    width: 50px;
	    height: 50px;
	    margin: 0 auto;
	    line-height: 0;
	    font-size: 0;
	    display: block;
	    padding: 0 0 8px;
	  }
	}

# Some hints

* The colors variable file should be a separate file

* Use Node JS
  *  Read a file met http://nodejs.org/api/fs.html#fs_fs_readfilesync_filename_options
  *  Write with fs.writeFileSync
  * Later we can automate this with grunt

* Node and jQuery https://github.com/coolaj86/node-jquery 
* http://rubular.com/ voor regex

Start off to read the file:

	var text = fs.readFileSync('colors.scss', 'utf8');

## Parts of the code

### Regex

A regex should look for colors, it should for variables and their respective colors, so it should match:

	($[*.]) // capture group 1 which is $ following any text ending with colon :
	[]    // bunch of spaces
	[]    // capture group 2 which starts after the spaces
	!default  ignore any mention of !default
	;    // one color per line

These colors should be saved to an array:

	['very-light-color', '#FFF'],
	['light-color', '#DDD'],

If the color uses a color function in Sass it should be parsed but this is a bit extensive for v1.

This array should then be written out in HTML form:

Write this to DOM: 

	<!-- start of template-->
	<div id="component-colors">
		<div class="colors">
	<!-- end start -->

Then write this to DOM:

? How to read from an array ?
		
    <!-- Loop for every color if the color is white add a box shadow -->

    <div>
        <div class="color-example" style="background: #FFF; box-shadow: 0 0 0 1px #DDD"></div>
        <p>$very-light-color: #FFF</p>
    </div>
    
    <!-- Else dump color value and add color name -->

    <div>
        <div class="color-example" style="background: #f5f6f7;"></div>
        <p>$light-color: #f5f6f7</p>
    </div>

The loop should use 
? Use a JS templating language for this part ?

Then write this:
		    
		</div>
	</div>