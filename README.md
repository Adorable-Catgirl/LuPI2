# LuPI2
Second attempt at Lua based operating system, primarily aimed at RaspberryPi, but with ambition to support other boards as well. The main motivation is 
fact that GNU/Linux + python solution isn't allays the best for people that haven't been programming ever, and Lua in one of the simplest, most 
intuitive languages. It has only 6 types, very simple syntax, yet supports many advanced mechanisms.

Build
-----
1. Clone this repository
2. Get musl cross compiler(like arm-linux-musleabihf), simplest way is to use [musl-cross](https://github.com/GregorR/musl-cross)
3. Get `xxd` utility (usually packaged with vim)
4. Build dependencies using scripts/dependencies.sh script for your platform(s)
5. Execute `make build`
6. You will need to put some OS to `root` directory where you run the binary. For now you can get plan9k at https://cloud.magik6k.net/index.php/s/7jPRAU037dzt8Ga/download

In case of problems poke me/someone at #lupi on Freenode

Idea
-----
Design of system APIs is heavily influenced by [OpenComputers](https://github.com/MightyPirates/OpenComputers) minecraft mod. Some Lua code parts are 
actually copied from there (all of the code is under the MIT License). Main advantage of the API is that it's event/component based, which provides great 
level of abstraction. Custom components can be created and used with very little effort, being event-based simplifies code further, providing one unified 
queue for events instead of multiple ways of handling them.
```lua
local component = require("component")

--Create virtual LED component using built-in GPIO component
local led = {}
led.toggle = function() component.gpio.togglePin(27) end
component.register(nil, "LED", led)

--Blink the LED
while true do
  os.sleep(1)
  component.led.toggle()
end
```

Implementation
-----
On the low-level side LuPI will run on very stripped-down version of Linux kernel as init, it will be the only binary executable in system. Kernel will 
only provide hardware drivers and abstract some of the things. Entire userspace is meant to be done using Lua. Security isn't the primary goal but still 
needs to be considered.

Get involved
-----
There are many ways you can help.
* Report issues
* Contribete via pull requests
* Talk on IRC (#lupi on Freenode)
