# Cheat Sheet - FIND - Image Manipulation Related Items

By Jack Szwergold, September 29, 2015

### Convert images into a JPEG tumbnail.

#### The process for just JPEG images.

Dry run to find any JPEG images and just echo their full path:

	find 'Desktop/Pics' -type f -name '*.jpg' |\
	  while read FULL_IMAGE_PATH
	  do
	    echo "${FULL_IMAGE_PATH}"
	  done

Convert any JPEG images found into JPEG thumbnail images at 90% quality:

	find -E 'Desktop/Pics' -type f -iregex '.*\.(JPG|JPEG|PNG|TIF|TIFF)$' |\
	  while read FULL_IMAGE_PATH
	  do
	    DIRECTORY=$(dirname "$FULL_IMAGE_PATH")
	    FILENAME="${FULL_IMAGE_PATH%.*}"
	    convert -density 72 -units PixelsPerInch -quality 90 "${FULL_IMAGE_PATH}" "${FILENAME}"_t.jpg
	  done

#### The process for any JPEG, PNG or TIFF images.

Dry run to find any JPEG, PNG or TIFF images and just echo their full path:

	find -E 'Desktop/Pics' -type f -iregex '.*\.(JPG|JPEG|PNG|TIF|TIFF)$' |\
	  while read FULL_IMAGE_PATH
	  do
	    echo "${FULL_IMAGE_PATH}"
	  done

Convert any JPEG, PNG or TIFF images found into JPEG thumbnail images at 90% quality:

	find -E 'Desktop/Pics' -type f -iregex '.*\.(JPG|JPEG|PNG|TIF|TIFF)$' |\
	  while read FULL_IMAGE_PATH
	  do
	    DIRECTORY=$(dirname "$FULL_IMAGE_PATH")
	    FILENAME="${FULL_IMAGE_PATH%.*}"
	    convert -density 72 -units PixelsPerInch -quality 90 "${FULL_IMAGE_PATH}" "${FILENAME}"_t.jpg
	  done

### Strip out all EXIF data with ExifTool.

Strip out all image EXIF data from JPEGs with ExifTool to:

	find 'Desktop/Pics' -type f -name '*.jpg' |\
	  while read FULL_IMAGE_PATH
	  do
	    exiftool -all= -overwrite_original_in_place "${FULL_IMAGE_PATH}"
	  done

Strip out all image EXIF data from JPEG, PNG or TIFF images with ExifTool to:

	find -E 'Desktop/Pics' -type f -iregex '.*\.(JPG|JPEG|PNG|TIF|TIFF)$' |\
	  while read FULL_IMAGE_PATH
	  do
	    exiftool -all= -overwrite_original_in_place "${FULL_IMAGE_PATH}"
	  done

Find any stray images with the `*.jpg_original` extension that ExifTool created and remove them:

	find 'Desktop/Pics' -type f -name '*.jpg_original' |\
	  while read FULL_IMAGE_PATH
	  do
	    rm "${FULL_IMAGE_PATH}"
	  done

***

Change the DPI of JPEG images 200 dpi:

	find 'Desktop/Pics' -type f -name '*.jpg' |\
	  while read FULL_IMAGE_PATH
	  do
	    FILENAME="${FULL_IMAGE_PATH%.*}"
	    convert -density 200 -units PixelsPerInch -quality 90% "${FULL_IMAGE_PATH}" "${FILENAME}".jpg
	  done

Resize JPEGs to dimensions of 1000 pixels wide or high if they are larger than 1000 pixels:

	find 'Desktop/Pics' -type f -name '*.jpg' |\
	  while read FULL_IMAGE_PATH
	  do
	    FILENAME="${FULL_IMAGE_PATH%.*}"
	    convert -density 72 -units PixelsPerInch -resize "1000x1000>" -quality 90 "${FULL_IMAGE_PATH}" "${FILENAME}".jpg
	  done

Resize JPEGs to dimensions of 1500 pixels wide or high if they are larger than 1500 pixels:

	find 'Desktop/Pics' -type f -name '*.jpg' |\
	  while read FULL_IMAGE_PATH
	  do
	    FILENAME="${FULL_IMAGE_PATH%.*}"
	    convert -density 72 -units PixelsPerInch -resize "1500x1500>" -quality 90 "${FULL_IMAGE_PATH}" "${FILENAME}".jpg
	  done
***

#### TIFF image processing

Strip out the EXIF ICC profile from all TIFF images:

	find 'Desktop/TIFFs' -type f -name '*.tif' |\
	  while read FULL_IMAGE_PATH
	  do
	    exiftool -icc_profile:all= -overwrite_original_in_place "${FILENAME}"
	  done

Change image density to 1200 DPI without reprocessing the image:

	find 'Desktop/JPEGs' -type f -name '*.jpg' |\
	  while read FILENAME
	  do
	    exiftool -all= -overwrite_original_in_place "${FILENAME}"
	  done

	find 'Desktop/JPEGs' -maxdepth 1 -type f -name '*.jpg' |\
	  while read FULL_IMAGE_PATH
	  do
	    DIRNAME=$(dirname "${FULL_IMAGE_PATH}")
	    BASENAME=$(basename "${FULL_IMAGE_PATH}")
	    convert -units PixelsPerInch "${DIRNAME}/${FILENAME}" -density 1200 -quality 100 "${DIRNAME}1200/${BASENAME}"
	  done

***

*Cheat Sheet - FIND - Image Manipulation Related Items (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*