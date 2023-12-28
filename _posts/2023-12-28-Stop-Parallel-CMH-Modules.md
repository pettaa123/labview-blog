In the world of inter-process communication techniques, Channeled Message Handlers (CMH) stand out as the visually engaging counterparts to the widely used Queued Message Handlers. They offer a new level of appeal and traceability to data flow across processes.

One key distinction lies in their structural boundaries. Unlike the tunnel-like structures of traditional methods, CMH employs pipish wires that elegantly cross structure borders. This immediate propagation upon message enqueuing deviates from the conventional tunnel propagation rules, marking a significant departure.

Lately, my focus has been on exploring CMH for application modularization. I’ve taken a different approach compared to the National Instruments CMH template by steering away from incorporating an "aborted channel" case structure around various message frames. My rationale behind this choice is to streamline CMH usage, emphasizing its cleanliness—an attribute that sets it apart from the tangled nature of QMH.

Instead of the aborted channel case, I’ve found a workaround by implementing an "Exit" Message. This message not only displays encountered errors leading to the exit but also triggers a shutdown signal to my module(s) and an Exit Event within the Event structure, ensuring a smooth stop.

![StopModuleInQMH](/labview-blog/assets/images/StopModulesInQMH.png)
