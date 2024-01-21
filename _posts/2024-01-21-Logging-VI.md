As projects expand, the need to log diverse events such as state changes, warnings, and execution times becomes crucial. To maintain a clean main VI without cluttering it with repetitive functionality, it's advantageous to encapsulate the logging process within a SubVI.

The goal is to design a logging VI that appends new entries to the log history while minimizing the impact on the main VI's connector pane. Efficiency is paramount, both in terms of input-output parameters and execution time. In pursuit of this, I've devised a solution: The FGV logger.

FGVs, or Function Global Variables, are globals with memory accessible only through functions, which prevents race conditions. The FGV logger VI is non-reentrant, ensuring a single instance of the VI is active at any given time. Data persistence between executions is achieved through an uninitialized shift-register.

What sets this logging VI apart is its adaptability for use in different VIs within an application, each with its own logging display. The FGV logger maintains an efficient map, initialized in the first call, that combines the logging history with the caller VI's name. For every VI utilizing the logger, a new map entry is added, ensuring seamless integration into diverse parts of the application.


![FGVlogger](/labview-blog/assets/images/FGVlogger.png)


