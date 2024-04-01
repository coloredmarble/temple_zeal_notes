# Hopefully useful notes for TempleOS/ZealOS

TempleOS -> ZealOS [changes][https://zeal-operating-system.github.io/Doc/ChangeLog.DD.html]

btw i still havent looked at all of it yes

notes:

i don't know shit currently. i barely know how to draw

function names appear to be swapped between templeos/zealos

from (verb)(noun) in templeos to (noun)(verb) in zealos

for example: GetKey in templeos; is KeyGet in zealos

i am still not sure if every function name is swapped like that  
but after taking a look at a few functions it probably is  

HolyC and ZealC is pretty much the same other than the renaming

The compiler messages on both SUCKS ASS. so you will more than likely spend hours fixing.
it wont even tell you where the error is sometimes

documentation is sparse. but is somewhat better in ZealOS.
you will HAVE to dig through the source to find out how to use stuff

ZealOS notes: The documentation for graphics in ZealOS is great. open "Help Index"/Graphics

when navigating. SPACE = left click. ENTER = right click

The Ed(); Function is the default editor. you will use it alot

keybinds i use alot:  
```
CTRL ALT T = open new term
CTRL ALT X = kill task
CTRL M = Open MENU in term
ALT A = AutoComplete
ALT SHIFT A =  Close AutoComplete
// AutoComplete
CTRL F(n) = AutoComplete with symbol at F(n)
CTRL SHIFT F(n) = Goto defenition of symbol at F(n)
// Ed;
CTRL S = save file when editing
F5 = run currently edited .HC/.ZC file
Varoom
```

### functions can be called without parenthesis (if no args or with optional args)
```
U0 Foo(I64 optional=0){
  Print("hewwo, %d",optional);
}
Foo;
```

note: when looking at templeOS tutorials while using ZealOS
and a symbol name is invalid. try swapping the noun and verb.
OR. search for a function with a similiar name with AutoComplete

## Drawing:
We'll draw a rectangle on screen
```

U0 DrawIt(CTask *task, CDC *dc){
  // set global canvas color
  dc->color = GREEN; // GREEN is a color global variable (value is 2)
  // draws a rectangle
  // (canvas,x,y,width,height)
  GrRect(dc,0,0,64,64);
}
```
where `CTask` will point to the given task (Fs). and `CDC` is the canvas
```
U0 Main(){
  // no idea what the fuck they do
  SettingsPush;
  DocClear;
  WinMax; // fullscreen
  // Fs points to the current task
  // set draw fn
  // btw draw loop is done concurrently
  Fs->draw_it = &DrawIt; // remember the & prefix
  // wait for any character
  while (!CharScan){Sleep(100;)}
}
// also REMEMBER to call main
Main;
```
btw there is an easier way to do this! if you want a non-updating draw (there are no moving jiffies)
```
U0 Main(){
  // DCAlias exists so that 2 tasks can draw to the screen with different pen colors. check src comments
  GrRect(DCAlias(),0,0,64,64);
}
```
however. even when the tasks that is drawing exists. the square will be stuck there until you clear it.
you can use `DCClear(gc.dr)` so it wipes the screen white for ya

and.. if you hate your entire screen being white. Call `DCFill`

so if you want a non-updating draw that doesnt get high on LCD or flashbang you. just call the function in main
and fill the screen later
```
U0 Main(){
  CDC* dc = DCAlias;
  dc->color = LTRED;
  GrRect(dc,128,128,64,64);
  // wait for any keypress
  PressAKey;
  DCFill;
}
Main;
```
