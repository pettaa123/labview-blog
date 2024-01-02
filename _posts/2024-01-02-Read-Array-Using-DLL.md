wrapper.h:
```
#include "lv_prolog.h"
#include "platdefines.h"

/* LabVIEW created typedef */
typedef struct {
    int32 dimSizes[2];
    uint16_t elt[1];
} Arr2D_U16;
typedef Arr2D_U16** Arr2D_U16Hdl;

#include "lv_epilog.h"

extern "C" BLOG_DLL_API int32_t readTif_U16(char* file, Arr2D_U16 lvArray);

```
wrapper.cpp
```
int32_t readTif_U16(char* file, Arr2D_U16 lvArray)
{
	try
	{
		std::string fname(file);

		// load range image
		cv::Mat mat;
		mat.load(fname);
		//make sure Mat is u16

		MgErr err = DSSetHandleSize(mat, mat.dataSz());
		if (err)
			return EXIT_FAILURE;

		memcpy((*lvArray)->elt, mat.data(), mat.dataSz());
		(*lvArray)->dimSizes[0] = mat.height();
		(*lvArray)->dimSizes[1] = mat.width();
		
	}
	catch (...)
	{
		return EXIT_FAILURE;
	}
	return EXIT_SUCCESS;
}
```
From time to time one needs to reads in data, which LabVIEW natively doesn't support. For me this was the case with 16U .tif images. Normally I would promote searching an adequate 3rd-Party toolkit in native LabVIEW code. I found some sources which claim that 16U .tif is not a specified format, which why not every tif is compatible with every software. This is why I preferred to use the read and write methods of the c++ library, which I use for the processing anyways.
A wrapper dll which fills a LabVIEW array with the read .tif is only reasonable to use, if LabVIEW needs no preknowledge about the amount of data to read in advance.
This is where ```DSSetHandleSize``` comes in handy. DSSetHandleSize belongs to LabVIEW's memory management functions and lets the DLL resize an array to the desired size, before returning it. This makes the data interaction between LabVIEW and a DLL seamingless and straightforward. The block diagram and the call library node look like this:

![ArrayResize](/labview-blog/assets/images/arrayResize.png)



