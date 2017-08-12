# Go-SDL2 Example
This repository contains a Go-SDL2 example code. It also aims to be a packaging guide and reference for various operating systems.

## Packaging
### macOS
1. Build the program using `go build`.

   ```
   $ go build
   ```

2. Find out the libraries you need besides the system libraries. You can find out using `otool -L <executable name>`. In this case, it would be `otool -L go-sdl2-example`. Output:

   ```
   $ otool -L go-sdl2-example
   go-sdl2-example:
   	/usr/local/lib/libSDL2-2.0.0.dylib (compatibility version 5.0.0, current version 5.1.0)
   	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1238.60.2)
   ```

3. Replace the library path in the executable using `install_name_tool`.

   ```
   $ install_name_tool -change /usr/local/lib/libSDL2-2.0.0.dylib @executable_path/../Frameworks/libSDL2-2.0.0.dylib go-sdl2-example
   ```

4. Create a directory such as `Go-SDL2-Example.app` and a subdirectory `Contents` inside. Inside the `Contents` directory, create `MacOS` and `Frameworks` directories. Put your executable `go-sdl2-example` inside the `MacOS` directory and the library it needs `libSDL2-2.0.0.dylib` inside `Frameworks` directory.

   ```
   $ mkdir -p Go-SDL2-Example.app/Contents
   $ mkdir Go-SDL2-Example.app/Contents/{MacOS,Frameworks}
   $ cp go-sdl2-example Go-SDL2-Example.app/Contents/MacOS
   $ cp /usr/local/lib/libSDL2-2.0.0.dylib Go-SDL2-Example.app/Contents/Frameworks
   ```

5. Create an `Info.plist` file inside the `Contents` folder with the following content:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
   	<key>CFBundleExecutable</key>
   	<string>go-sdl2-example</string>
   	<key>CFBundleIdentifier</key>
   	<string>co.veand.gosdl2.example</string>
   	<key>CFBundleVersion</key>
   	<string>1.0</string>
   	<key>CFBundleDisplayName</key>
   	<string>Go-SDL2 Example</string>
   	<key>LSRequiresIPhoneOS</key>
   	<string>false</string>
   </dict>
   </plist>
   ```

You can find out more about `Info.plist` at https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html.
