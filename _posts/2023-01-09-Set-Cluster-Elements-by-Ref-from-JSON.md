Reading JSON files into LabVIEW to populate clusters offers efficiency by integrating diverse data sources seamlessly. It streamlines processes, enhances compatibility with web-based systems using JSON, and automates data handling, enabling easier analysis within the LabVIEW environment.

To improve code reusability, it's beneficial to avoid specific clusters as subVI input parameters. Opting for passing clusters by reference rather than by value enhances reusability. However, working with generic cluster references can sometimes be cumbersome. The example below efficiently resolves this issue.

![PCD-ViewerFP](/labview-blog/assets/images/SetClusterValuesByRef.png)


