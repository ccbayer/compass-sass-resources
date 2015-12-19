# compass-sass-resources
Mixins, Plugins, etc


# SASS Themeing

The requirements for a project were to create components that could be any number of a number of color themes (e.g, "Blue") and then any number of tones under that theme (e.g, "light blue", "dark blue"). So for example, a single widget may be reused throughout the site but have a light blue heading on one page, and a dark blue heading on another page.

To effectively create a list of utility classes that abstracted the color from the class of the component and be used on any element of my choosing, I used a combination of Sass maps and loops.

1. Define list of theme colors and tones:
``` example: Dark Blue, Medium Blue, Light Blue ```
2. Generate variables and a map for this set of colors:
````
$darkblue: #0B2D71;
$medblue: #0066b2;
$lightblue: #009DD9;
$themeblue: (
	'darkblue': $darkblue,
	'medblue': $medblue,
	'lightblue' : $lightblue
);
````

The variables are not *required* in this case, but are useful for singletons / one-offs that may be used elsewhere in your SCSS.

3. Create a "theme" mapping, which references the individual theme map to its tonal values:
```
$themes: (
	'blue': $themeblue,
	'red': $themered
);
```

Visually, this sort of charts out like this:

```
Theme Blue
		|___Dark Blue: #0B2D71;
		|___Medium Blue: #0066b2;
		|___Light Blue: #009DD9;
```		

For optimal use, you'd loop over the themes, then subloop over that theme's tones, to create utility classes for all the colors in your theme:


```
@each $theme, $colors in $themes { // for each individual theme, and that theme's set of colors in the list of themes...
	@each $currentColorTone, $currentColor in $colors { // for each individual current color tone and its corresponding hex in the list of colors applied to that theme...
		.bg-#{$currentColorTone} { // define a class that maps to a and is name-spaced to the current color tone  
			background-color: $currentColor; // use the current color as the value for the CSS property you want. This one is used for background color.
		}
		.color-#{$currentColorTone} { // defines a color class to be used to change text color
				color: $currentColor;
		}
	}
}
```
In my project, it became useful with mixins as well, for example, creating different buttons:
```
		.btn-#{$currentColorTone} {
			@include button-variant($white, $currentColor, $currentColor);
		}
```

And defining border colors:
```
		.border-#{$currentColorTone} {
			border-color: $currentColor;
		}
```

For a fully fleshed out example, see the `_theme.scss` file.

