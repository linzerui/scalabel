# Scalable Anntoation Tooling

## Overview ##
SAT is a versatile and scalable tool that supports all kinds of annotations needed in a driving database, such as bounding box, semantic instance segmentation, and lane detection.  
A screenshot of bounding box annotation interface:
![alt text](/example/bbox_tool.jpg)

## Initial Setup ##
* Create a directory to store image annotations: 
```
mkdir ../data
```
* Install Golang by building and running a Docker image from the 
Dockerfile.  
e.x.
```
docker build .
```

## Images and Labels ##
* Create **label.txt** with a list of object categories you wish to label. 
Refer to examples in /example/bbox_label.txt and /examples/drivable_label.txt. 
* Create **image_list.json** with a list of paths to image files. These images 
should be publicly accessible or stored in the server. Refer to example in
/example/image_list.json

## Running Server ##
Specify a port to start the server and a directory path to store 
image annotations. Use the following command to run the server:
```
go run main.go --port 8686 --data_dir "../data"
```

## Navigating the tool ##
We'll demonstrate how to navigate our tool. Here, we default to localhost and 
listen to port 8686.

1. Go to http://localhost:8686/create to create a new task. You need to 
upload label.txt and image_list.json you just created, and specify other 
task configurations such as project name, task size, label type, and vendor ID. 
* **Project name**: You can find all task assignments in 
<data_dir>/Assignments/<project name>/, completed annotations in  
<data_dir>/Submissions/<project name>/, and log files in 
<data_dir>/Logs/<project name>/
* **Task size**: You can specify number of images to be labeled for each assignment.
* **Label type**: We provide five different label types: '2d_bbox', '2d_road', '2d_seg', '2d_lane','image'.
* **Vendor ID**: This keeps track of which vendor/annotator to be assigned to. 
You can specify '0' if unsure.

2. Once you click '**enter**', '**go to dashboard**' will direct you to 
http://localhost:8686/dashboard?project_name=<project 
name>... 
* From here, you can access and monitor list of tasks 
created. 
* You can download a list of task URLs, and send them to your 
vendor. Vendors can concurrently access and work on one task URL. 
* You can download all result of annotation at anytime.

Here is a labeling tool workflow diagram:
![alt text](/example/workflow.jpg) 

## Directories ##
* app folder contains front-end scripts for control panel, bounding box, 
image level, drivable area, lane markings and segmentation.
* example folder contains task examples.
* main.go is the back-end go-lang server script.

## How to Use Region Annotation ##
Region Annonation consists of annotations of drivable area, lane markings and image segmentation, corresponding to label types of "2d_road", "2d_lane", and "2d_seg".
* When you open a region annotation task, you will notice there is a tool box on the left of the screen. 
* Click along the edge of an object to draw a mask. To finish drawing, go back to the first vertex (where you started) to form a closed path.
* By pressing "ESC", you can remove an unfinished object.
* You can always double click an object and change its category in this toolbox, or click "delete" to remove it (or press "delete" on your keyboard).
* You can change the shape of an object by dragging its vertices. If you hover your mouse on these points, they will become green and bigger.
* You can delete a vertex by hovering and then pressing "delete".
* You can add a vertex by dragging the corresponding midpoint (in pink). The midpoint will become a new vertex after this operation, and two other midpoints will be generated. The corresponding midpoint is in orange when you hovering your mouse on it.


A screenshot of vertexs and midpoints:


![alt text](/example/vertex.png)![alt text](/example/midpoint.png)


* You can use a local magnifier by choosing "Magnify" in the toolbox. Or you can press "PageUp/PageDown" on your keyboard or click "+/-" on top of the webpage to zoom in/out.
* Add Bezier Curve: Hover your mouse on a midpoint, then press "**B**" or "**b**" on the keyboard. Then it will split to two points on the corresponding edge. And you can drag these two points and another two endpoints to change the shape of the Bezier Curve.
* Delete Bezier Curve: Hover your mouse on a control point of the Bezier Curve, then press "delete" on the keyboard. It will become a straight edge.


## Some Additional Options for Segmentation Annotation ##
* **Copy Boundary**  
1. When you want to draw the coincide part of two objects, press "**S**" or "**s**" before or during drawing;
2. Then you can notice the polygon is no longer filled, and you can also see the vertices of other objects when moving your mouse onto them. That means you have entered the "quick draw" mode. Meanwhile, the toolbox will become blue. It will remain blue or green unless you exit the "quick draw" mode. (By pressing "S/s" again).
3. Select two vertices on the border by clicking. After your first click, the toolbox will turn green, and the object you selected will change color. After your second click, the toolbox will change from green to blue. You can see a polyline is formed.
4. You can use "PageLeft/PageRight" on your keyboard to choose the border, because there are two directions: clockwise or counter-clockwise.
5. The toolbox will remain green before you click a pair of points on the same object. And when you click on a new object, it will remain green, since no coincide border is formed, so that you can continue finding its pair.
6. Click the background image, the toolbox will be blue. So in conclusion, color green means you have finished the first click and should look for the second point; color blue means your should look for the first point.
7. The same as ordinary draw mode, press "delete" to delete the latest vertex or last added border, and press "esc" to delete the whole object. And when a closed path is formed, your annotation is finished.
8. You can use it to click anywhere like the ordinary draw mode, it only provides function to draw coincide part quickly by clicking twice.
* **Link**  
To link separated parts of an object into one with the same object ID
1. You can select one part of the object (by double click), and click "link" button, then single click the others, and click "finish link".
2. You can also click "Link" first, then single click every object to be linked, and then click "finish link".
