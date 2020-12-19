[Procedural texture](https://en.wikipedia.org/wiki/Procedural_texture) 
* 2D [color gradient](https://en.wikipedia.org/wiki/Color_gradient)
* [defined by a function whose value is computed rather than looked up](http://math.hws.edu/graphicsbook/c7/s3.html)






# images

## gray = 8 bit color ( c images ) 

### cabs(z)
![](./images/conic.png "conic") 
![](./images/cabs.png "cabs") 
![](./images/cabsi.png "cabs inverted") 

### carg(z)
![](./images/carg.png "carg") 
![](./images/cargm.png "carg modified") 
![](./images/cturn.png "cturn") 

### max min

![](./images/max.png "max") 
![](./images/min.png "min") 

### star 8
![](./images/star8.png "star8") 
![](./images/star8i.png "star8 inverted") 

### himmelblau function
![](./images/himmelblau.png "himmelblau function") 

### checkerboard
![](./images/checker.png "checker")
 


## pascal images (up to 24 bit color)

![](./images/absz.jpg "abs") 
![](./images/b.jpg "b") 
![](./images/c.jpg "j") 

![](./images/e.jpg "e") 
![](./images/g.jpg "g") 
![](./images/h.jpg "h") 
![](./images/hsv.jpg "hsv") 

### max(x,y)
![](./images/d.jpg "d") 
![](./images/maxreim.jpg "maxreim") 

### xor

![](./images/xxory.png "k = x xor y, 24 bit color") 

### sin 



#### sin(x*y)
![](./images/l.jpg "k = sin(x*y); single pattern ,  24 bit color") 
![](./images/sinxmy.png "k = sin(x*y); repeating pattern, 24 bit color") 

#### sin(x) + sin(y)
![](./images/sin.jpg "k =  sin(x) + sin(y), rgb = 24 bit color") 
![](./images/singray.jpg "k =  sin(x) + sin(y), gray = 8 bit color") 
![](./images/sinbw1.jpg "k =  sin(x) + sin(y), black and white = 1 bit color") 
![](./images/Jacco179SinXSinY.jpg "k =  sin(x) + sin(y), indexed color ( palette)") 


#### sin(x) + sin(y) + sin(x+y)

![](./images/sinxyxy.png "k = sin(x) + sin(y) + sin(x+y),  indexed color from palette") 
![](./images/sinbw.jpg "k = sin(x) + sin(y) + sin(x+y), black and white = 1 bit color ") 

### whirl
![](./images/whirl.jpg "whirl in 24 bit color") 




# Theory
[Procedural textures are 2D or 3D plots of mathematical functions (called procedures)](https://knowledge.autodesk.com/support/maya/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Maya/files/GUID-B2C969C0-48CD-45AB-8C7B-E6FC9E34AD19-htm.html) 



Parts
* 2D [scalar field](https://en.wikipedia.org/wiki/Scalar_field) ( 2D array , image, [complex plane](https://en.wikipedia.org/wiki/Complex_plane)  )
* color
  * modes
  * function f : (R x R) maps to  color
* plane transformations
* Texture Mapping = repeatedly paint the same small picture onto your geometry   
  
  
  
  

## color modes

[Color depth](https://en.wikipedia.org/wiki/Color_depth)
* 24 bit color (rgb) 
* 8 bit color mode: gray shades ( where r=g=b)
* 1 bit color mode: Black and White ( b&w )
* direct color, palette mode

```pas
 case ColorType of
                    TrueColor: if FunctionType=HSV   // 24 bit color
                    	then kolor:=k 
                    	else kolor:=Rainbow(kMin,kMAx,k mod kmax);
                    Direct:    kolor:=k;
                    GrayScale: kolor:=GrayScaleF(Round((k*256) div kmax)); // 8 bit color
                    Pseudo8bit:kolor:=GivePseudo8bitColor(k mod 255);
                    BlackAndWhite: if odd(k) then kolor:=clBlack // 1 bit color
                                         else kolor:=clWhite;
                  end; // case  ColorType }
```





## color functions


Function
* input: the texture coordinates: ( x,y)
* output: a color value



# Color functions
## pascal
For color functions in pascal ( Delphi) see file: [ColorM.pas](./src/pas/ColorM.pas)

Here input is a pair of integers (x,y): 
* x in [0, bitmapa.width-1]
* y in [0, bitmapa.height-1]


```pascal
Function Projection(center:TPoint;height:integer;x,y:integer;FunctionType:TFunctionType):integer;
  // F: C --> R
  //var r:extended; //radius
  begin

     case FunctionType of
            HSV:        result:= Point2Color(X,y);
            AbsZ:       result:=round(sqrt(sqr(X-center.X)+sqr(y-center.Y)));
            ArgZ:       result:=round(RadToDeg(Pi+ArcTan2(y-center.Y,center.X-x)));
            Whirl:      result:=round(sqrt(sqr(X-center.X)+sqr(y-center.Y))+RadToDeg(2*Pi+ArcTan2(y-center.Y,center.X-x)));
            ReZ:        result:=y ;  // horizontal lines
            Saddle:     result:=abs(sqr(x-center.X)-round(sqr(y-center.y) / 2)) ;
            RePlusIm:   result:=y+x;
            MaxReIm:    result:=max(abs(center.x-x),abs(center.Y-y)); // or
            AbsReIm:    result:=abs((center.x-x)*(center.Y-y));           // biomorph
            ImDivRe:    result:=floor(tan((center.Y-y) / (center.X-x+0.000001)));  //
            ImReDiv:    result:=y+x+(100*y div (x+1));
            manhattan:  result:=sqr(abs(center.X-x)+abs(center.Y-y));                                                              // ClientHeight*(sin+1)/2= [0,ClientHeight]

            sinusX:     result:=round(bitmapa.Width*sin(Pi*DegToRad(x)));
            Sinus:      result:=y+round(height*(sin(Pi*DegToRad(x))+1)/8);
            // sin   is in  [-1,+1]
            // (sin+1)      [0,2]
            // (sin+1)/2    [0,1]
            SinusXY:    result:=y+round(height*(sin(Pi*DegToRad(x + y))+1)/8);
            SinusXmY:   result:=y+round(height*(sin(Pi*DegToRad(x*y))+1)/8);
            SinXSinY:   result:=round((height * (2+sin(Pi*DegToRad(y)) + sin(Pi*DegToRad(x)))) /4);//try to change numerical values
            sinXYXY:     result:=round ( height * (2+sin(Pi*DegToRad(y)) + sin(Pi*DegToRad(x))+sin(Pi*DegToRad(x+y)))/8 );
            XorY:         result:=x or  y;
            XxorY:        result:=x xor y;
            XshlY:        result:=x shl y;
            XshrY:        result:=x shr y;
            XandY:        result:=x and y;
            sqrtM:        result:=round(sqrt(x*y)) ;
            ftMax:        result:=Round((center.Y-y) / sqr(center.X -x + 0.0001));
            plasma: result:=round(bitmapa.Width*sin(Pi*DegToRad(x)));
            else          result:=y; //
          end; // case
  end;

```
## c
Here input is a pair of doubles z = (x,y) 
* x in [-1,1]
* y in [-1,1] 


```c
double conic(double complex z)
{
  double argument;
  
  argument = carg(z); //   argument in radians from -pi to pi
  
  argument = fabs(argument)/ M_PI;
      
  return argument; // argument in range from 0.0 to 1.0
}
```

```c

// https://en.wikipedia.org/wiki/Himmelblau%27s_function
double GiveHimmelblau(double x, double y){
	// mapped input to [-6,6]x[-6,6]
	x *= 6.0;
	y *= 6.0;
	double a = x*x+y-11.0;
	double b = x+y*y-7.0;
	// mapped output to 
	 return (a*a + b*b)/200.0;

}
```


```c
/*
	https://iquilezles.org/www/articles/checkerfiltering/checkerfiltering.htm
	checkers, in mod form
*/
double checker( double x, double y){

	int ix = floor(5.0*x);
	int iy = floor(5.0*y);


	return abs(ix + iy) % 2;
}
```

```c
/* 
 r is the smooth potential and phi is the final angle
 code by xenodreambuie : "I call this texture pyramids. My code in Pascal for the Star8 texture is "
 
 
*/ 

double GiveStar8(double r, double phi){
	double fr;
	double fphi;
	double t;
	double g;
	
	fr = fabs(frac(r));
  	fphi = fabs(frac(phi));
  	if (fphi>fr) {
    		t= fr; 
    		fr= fphi; 
    		fphi=t;
    		}
  
	g = 1+1.5*fphi-2.5*fr;
	t = 1-2.5*fphi-fr;
	if (t> g) 
	  	{ g = t;}
	if (g<0)
  		{g=0;}
  	
	return g;
}
```

The checkerboard pattern is regular grid of equal-sized colored squares


# files
* [ColorM.pas](./src/pas/ColorM.pas)  
* [Rainbow.exe](./exe/Rainbow.exe) - windows executable program, can be run also on linux
* [d.c](./src/c/d.c) - c version of Rainbow program
* [g.sh](./src/c/g.sh) bash script for converting ppm files to png


# See also
## functions
* [island-gradient by Code 2D](https://code2d.wordpress.com/2020/07/21/island-gradient/)
* [2D distance functions by inigo quilez ](https://www.iquilezles.org/www/articles/distfunctions2d/distfunctions2d.htm)
* [a-simple-procedural-texture-algorithm by by Herman Tulleken ](http://www.code-spot.co.za/2008/11/07/a-simple-procedural-texture-algorithm/)
* [1D color gradient](https://gitlab.com/adammajewski/color_gradient)
* [ptdesigner](https://github.com/drummyfish/ptdesigner) by drummyfish
* [texture-studio ](http://www.mgsx.net/product/software/2015/09/18/texture-studio.html)
* [create-repeating-patterns-with-cycles-procedural-textures](https://blender.stackexchange.com/questions/26692/how-do-i-create-repeating-patterns-with-cycles-procedural-textures)
* [Sample Procedural Textures from ](https://docs.bentley.com/LiveContent/web/MicroStation%20Help-v13/en/GUID-AC1EEF76-9118-FE2E-32F6-399AF454E86C.html)
* [Textures by  David J. Eck](http://math.hws.edu/graphicsbook/c7/s3.html)
* [what-is-procedural-workflow](https://www.imanvfx.com/what-is-procedural-workflow/)
* [2D textures in Maya](https://knowledge.autodesk.com/support/maya/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Maya/files/GUID-EA9BC7E0-22B4-46C1-8D5B-3A4C8B680952-htm.html#GUID-EA9BC7E0-22B4-46C1-8D5B-3A4C8B680952)
* [A-generic-lattice-noise-algorithm-an-evolution-of](https://www.codeproject.com/Articles/785084/A-generic-lattice-noise-algorithm-an-evolution-of)


## 2D noise
* [Pocedural_Noise(2f)](http://physbam.stanford.edu/cs448x/old/Procedural_Noise(2f)Perlin_Noise.html)
* [FastNoise by Auburn](https://github.com/Auburn/FastNoise/)


## transformations 
* [imitate-a-textured-sphere-in-2d](https://gamedev.stackexchange.com/questions/147193/imitate-a-textured-sphere-in-2d)




# run

## pascal

On linux ( tested on Ubuntu 20.04): 
```
wine ./Rainbow.exe
wine ./exe/Rainbow.exe
```


Use: 
* open main menu, choose options item
* choose color mode
* use arrow up and down keys to change functions
* check the options in the status bar




# History

![](./images/delphi.gif "delphi logo")   

Old ( but still interesting) Pascal ( Borland Delphi 7.0 personal edition )  program Rainbow for windows ( but can also be run on Linux using wine)

dead old www address: fraktal.republika.pl/tecza.html

Last modification: 2005-05-29 





# Licence and contributors
* [licence](LICENCE) - GNU General Public License v3.0
* function [star8 by xenodreambuie ( pascal) ](https://fractalforums.org/programming/11/how-many-different-ways-are-there-to-show-such-set/3874/msg25389#msg25389)
* FUNCTION HSVtoColor from UNIT HSVLibrary ( efg, July 1999  www.efg2.com/lab  Copyright 1999, All Rights Reserved. May be used freely for non-commercial purposes.
* Function Rainbow by  Witold J.Janik; WJJ@CAD.PL thx for  Andrzeja W¹sika from pl.comp.lang.delphi  http://4programmers.net/view.php?id=201
  

# Git


create a new repository on the command line

```git
echo "# Procedural-texture" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/adammaj1/Procedural-texture.git
git push -u origin main
```               



## Subdirectory

```git
mkdir images
git add *.png
git mv  *.png ./images
git commit -m "move"
git push -u origin main
```
then link the images:

```txt
![](./images/n.png "description") 

```

```git
gitm mv -f 
```

## markdown
* [mastering-markdown](https://guides.github.com/features/mastering-markdown/)

## authenticate

[automatically-authenticate-into-github](https://stackoverflow.com/questions/28298861/how-to-automatically-authenticate-into-github-from-git-bash-using-my-public-and)  

```git
git remote set-url origin git@github.com:adammaj1/Procedural-texture.git
```
