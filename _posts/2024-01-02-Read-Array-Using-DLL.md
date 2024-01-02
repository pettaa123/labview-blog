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
Occasionally, there's a need to handle data in LabVIEW that isn't inherently supported by the platform. I encountered this when dealing with 16U .tif images. Typically, I'd suggest exploring third-party toolkits within LabVIEW, but in this case, the 16U .tif format isn't universally supported, leading to compatibility issues across different software.

To address this, I opted to leverage the read and write functionalities of a C++ library that I already use for image processing. To facilitate the integration with LabVIEW, I created a wrapper DLL. However, for LabVIEW to seamlessly handle data without requiring prior knowledge of the data size, I employed the DSSetHandleSize function. This function, part of LabVIEW's memory management capabilities, allows the DLL to resize an array to the required size before passing it back, ensuring smooth and direct data interaction between LabVIEW and the DLL.

The block diagram and the call library node were structured to support this seamless data exchange, enhancing the overall workflow.

![ArrayResize](/labview-blog/assets/images/arrayResize.png)

![ArrayResizeCLN](/labview-blog/assets/images/arrayResizeCLN.png)



