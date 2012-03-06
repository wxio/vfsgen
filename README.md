## Bindata

This tool converts any file into managable Go source code. Useful for embedding
binary data into a go program. The file data is gzip compressed before being
converted to a raw byte slice.

### USAGE

 The simplest invocation is to pass it only the input file name.
 The output file and code settings are inferred from this automatically.

    $ bindata -i testdata/gophercolor.png
    [w] No output file specified. Using 'testdata/gophercolor.png.go'.
    [w] No package name specified. Using 'main'.
    [w] No function name specified. Using 'gophercolor_png'.
    [i] Done.

 This creates the "testdata/gophercolor.png.go" file which has a package
 declaration with name 'main' and one function named 'gophercolor_png'.
 It looks like this:

     func gophercolor_png() ([]byte, error) {
	      gz, err := gzip.NewReader(bytes.NewBuffer([]byte{
              ...
          }))
          
          if err != nil {
              return nil, err
          }

          var b bytes.Buffer
          io.Copy(&b, gz)
          gz.Close()
          
          return b.Bytes(), nil
     }

 You can now simply include the new .go file in your program and call
 gophercolor_png() to get the uncompressed image data.
 See the testdata directory for example input and output.

 Aternatively, you can pipe the input file data into stdin. bindata will then
 spit out the generated Go code to stdout. This does require explicitly naming
 the desired function name, as it can not be inferred from the input data.
 The package name will still default to 'main'.
 
     $ cat testdata/gophercolor.png | ./bindata -f gophercolor_png | gofmt

 Invoke the program with the -h flag for more options.

