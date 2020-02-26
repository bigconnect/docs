# Images

## image.manual

Annotate images by drawing rectangular bounding boxes and polygon shapes. Each shape will be added to the task's `"spans"` with its label, colour and a `"points"` property containing the `[x, y]` pixel coordinate tuples. Rectangular shapes can be added by clicking or dragging. Polygon shapes can be closed by clicking on the start point, or by double-clicking anywhere on the image. To select an existing shape, click on its label or bounding box. Selecting a shape allows you to change its label or delete it from the image.

```text
$ prodigy image.manual image_objects /tmp/my_images/  --label PERSON,ANIMAL,CAR

✨ Starting the web server on port 8080...
```

## image.test

Test Prodigy's built-in object detection interface using one of the [YOLOv2](https://pjreddie.com/darknet/yolo/) object detection models via the [LightNet](https://github.com/explosion/lightnet) library. The recipe will load in images from a directory or API, and create one task per detected object.

```text
$ pip install lightnet
$ python -m lightnet download tiny-yolo
$ prodigy image.test image_objects tiny-yolo /tmp/my_images/

✨ Starting the web server on port 8080...
```

![](../../../.gitbook/assets/image%20%28546%29.png)



