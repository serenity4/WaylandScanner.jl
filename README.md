# WaylandScanner.jl
A Julia module for parsing Wayland protocol files and :unicorn:generating Julia bindings. In the future this module will hopefully be able to generate full Wayland bindings that can be statically compiled into something that doesn't even require a Julia runtime.

Heavily work in progress.

| Functionality | Status |
| ------------- | ------ |
| parsing XML protocol files | mostly done
| pretty-printing the parsed tree | mostly done
| generating Julia bindings | not even started

## Installing
```julia-repl
pkg> add https://github.com/Deuxis/WaylandScanner.jl
```
## Usage
Import the module.
```julia-repl
julia> import WaylandScanner
```
To parse a protocol file use `wlparse("path/to/file")`. This returns a `Set` of `SProtocol` objects, which represent the abstract tree of the protocol. The entire tree is documented in code and can be pretty-printed by the Julia REPL. For convenience, there is also a `wlparse()` method which runs `wlparse("/usr/share/wayland/wayland.xml")`, parsing the main protocol file at its default location.
```julia-repl
julia> WaylandScanner.wlparse("/usr/share/wayland/wayland.xml")
```
To easily parse a bunch of files you can make any collection of file paths and use a vectorized call `wlparse.(myfiles)`, then reduce the resulting collection of sets by `append!(col1, col2)`, resulting in one big set containing all the protocols.
```julia-repl
julia> filenames = ["/usr/share/wayland/wayland.xml", "/usr/share/wayland-protocols/stable/xdg-shell", "/usr/share/wayland-protocols/unstable/fullscreen-shell"]
3-element Array{String,1}:
 "/usr/share/wayland/wayland.xml"                        
 "/usr/share/wayland-protocols/stable/xdg-shell"         
 "/usr/share/wayland-protocols/unstable/fullscreen-shell"

julia> protocols = reduce(append!, wlparse.(filenames))
```

## Dependencies
The only direct dependencies are a working 1.0-compatible Julia runtime and the imported modules, LightXML and FixedPointNumbers, but LightXML depends on libxml2.

## Additional notes
LightXML seems to have some bugs interacting with my version of libxml2, resulting in occasional errors. Just re-run the wlparse function if that happens.
