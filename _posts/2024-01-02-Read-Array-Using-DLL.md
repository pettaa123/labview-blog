wrapper.h:
```
#pragma once
#include "fundtypes.h"
#include "extcode.h"

#ifdef CX3DLIBDLL_EXPORTS
#define CX3DLIBDLL_API __declspec(dllexport)
#else
#define CX3DLIBDLL_API __declspec(dllimport)
#endif

#include "lv_prolog.h"
#include "platdefines.h"

/* LabVIEW created typedef */
typedef struct {
    int32 dimSizes[2];
    uint16_t elt[1];
} Arr2D_U16;
typedef Arr2D_U16** Arr2D_U16Hdl;

#include "lv_epilog.h"

extern "C" BLOG_DLL_API int32_t readTif_U16(char* file, Arr2D_U16 lvAllocatedArray);

```
wrapper.cpp
```
int32_t readTif_U16(char* file, Arr2D_U16Hdl arr) {
	try
	{
		std::string fname(file);

		// load range image
		AT::cx::Image img;
		img.load(fname);
		//make sure image is u16

		MgErr err = NumericArrayResize(uW, 1L, (UHandle*)&arr, img.dataSz());

		if (err)
			return err;

		MoveBlock(img.data(), (*arr)->elt, img.dataSz());
		(*arr)->dimSizes[0] = img.height();
		(*arr)->dimSizes[1] = img.width();
	}
	catch (...)
	{
		return EXIT_FAILURE;
	}
	return EXIT_SUCCESS;

}
```
Occasionally, there's a need to handle data in LabVIEW that isn't inherently supported by the platform. I encountered this when dealing with 16U .tif images. Typically, I'd suggest exploring third-party toolkits within LabVIEW, but in this case, the 16U .tif format isn't universally supported, leading to compatibility issues across different software.

To address this, I opted to leverage the read and write functionalities of a C++ library that I already use for image processing. To facilitate the integration with LabVIEW, I created a wrapper DLL. However, for LabVIEW to seamlessly handle data without requiring prior knowledge of the data size, I employed the NumericArrayResize function. This function, part of LabVIEW's memory management capabilities, allows the DLL to resize an array to the required size before passing it back, ensuring smooth and direct data interaction between LabVIEW and the DLL.

The block diagram and the call library node were structured to support this seamless data exchange, enhancing the overall workflow.

![ArrayResize](/labview-blog/assets/images/arrayResize.png)

![ArrayResizeCLN](/labview-blog/assets/images/arrayResizeCLN.png)



