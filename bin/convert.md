# convert

## create pdf from images

```
 convert *.jpg -compress jpeg filename.pdf
```

## Install Required libraries:

```
sudo apt-get update && sudo apt-get install imagemagick gs
```

## create directory to test commands
```
mkdir -p pdf_conversion/{merged,split}
cd pdf_conversion
```

## download sample multi-page pdf to test with
```
curl -o LargeSample.pdf "http://cl.ly/0o1T453E153m/download/LargeSample.pdf"
```

## Split a pdf into separate images
```
convert LargeSample.pdf split/page-%0d.jpg
```

## Merge separate images into one full pdf
```
convert split/*.jpg merged/full.pdf
```

```
convert           \
    -density 1200
    06\ 2019\ Nominas\ QL.pdf      \
    -resample 300 \
   ./nomina-%0d.pdf
```

## Remove a page from a full pdf
```
convert LargeSample.pdf split/page-%0d.jpg
rm split/page-2.jpg
convert split/*.jpg merged/full.pdf
```

### Links to read:

- http://linuxcommando.blogspot.com/2015/03/how-to-merge-or-split-pdf-files-using.html
- http://linuxcommando.blogspot.com/2013/02/splitting-up-is-easy-for-pdf-file.html
- http://stackoverflow.com/questions/17025515/converting-a-multi-page-pdf-to-multiple-pages-using-a-single-command
- http://codetheory.in/convert-split-pdf-files-into-images-with-imagemagick-and-ghostscript/