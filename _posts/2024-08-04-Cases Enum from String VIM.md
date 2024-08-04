When dealing with case structures across a code boundary, it's useful to know which cases the structure supports. One effective solution is using enum typedefs. These help avoid errors by preventing the use of unsupported case instructions. However, when communicating via Messages, such as in CMH or QMH, the message typically consists of a string for the instruction and a variant for the payload. To utilize enum typedefs effectively in this context, a subVI can be used for bundling and unbundling messages.
Malleable VIs are perfect for this, as they reduce the number of necessary VIs to just one, regardless of how many different case structure typedefs you might create.
Without this approach, you would need to create numerous VIs for each case structure used.

![FGVlogger](/labview-blog/assets/images/Case VIM.png)
![FGVlogger](/labview-blog/assets/images/BundleMessage.png)
![FGVlogger](/labview-blog/assets/images/UnbundleMessage.png)


