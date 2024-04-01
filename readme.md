# Hopefully useful notes for TempleOS/ZealOS

TempleOS -> ZealOS [changes][https://zeal-operating-system.github.io/Doc/ChangeLog.DD.html]

btw i still havent looked at all of it yes

a few things:

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

remembered keybinds list:  
```
CTRL ALT T = open new term
CTRL ALT X = kill task
CTRL M = Open MENU in term
ALT A = AutoComplete
ALT SHIFT A =  Close AutoComplete
// AutoComplete
CTRL F(n) = AutoComplete at F(n)
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
  dc->color = GRREN; // GRREN is a color global var value(2)
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
  Fs->draw_it = &DrawIt; // remember the & prefix
  // at this point. the main function usually accept keys.
  I64 ScanKey; // calling KeyGet with args is optional. although i have no fucking idea why or why not
  while (TRUE){
    // GetKey for templeOS. KeyGet for ZealOS
    switch(KeyGet(&ScanKey)){ // KeyGet is blocking. so no need to sleep
        // esc key
        case CH_ESC: goto done; break;
    }
  }
  // HolyC have no "break"
  done:
  // exit
  Exit;
}
// also REMEMBER to call main
Main;
```
