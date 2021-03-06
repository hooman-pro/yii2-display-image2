Yii2 Display Image2
===================

Installation
------------
The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist pavlinter/yii2-display-image2 "dev-master"
```

or add

```
"pavlinter/yii2-display-image2": "dev-master"
```

to the require section of your `composer.json` file.

Configuration
-------------
```php
'modules' => [
  'display2'=> [
      'class'=>'pavlinter\display2\Module',
      'categories' => [
          'all' => [
              'imagesWebDir' => '@web/display-images/images',
              'imagesDir' => '@webroot/display-images/images',
              'defaultWebDir' => '@web/display-images/default',
              'defaultDir' => '@webroot/display-images/default',
              'mode' => \pavlinter\display2\objects\Image::MODE_OUTBOUND,
          ],
          'items' => [
              'generalDefaultDir' => false,
              'imagesWebDir' => '@web/display-images/items',
              'imagesDir' => '@webroot/display-images/items',
              'defaultWebDir' => '@web/display-images/default/items',
              'defaultDir' => '@webroot/display-images/default/items',
              'mode' => \pavlinter\display2\objects\Image::MODE_STATIC,
          ],
      ],
 ],
],
'components' => [
  'display' => [
    'class' => 'pavlinter\display2\components\Display',
    'resizeModes' => [
        'ownResizeMode' => 'pavlinter\display2\objects\ResizeMode',
        'ownResizeModeParams' => [
            'class' => 'pavlinter\display2\objects\ResizeMode',
        ],
        'ownResizeModeFunc' => function ($image, $originalImage) {
            /* @var $this \pavlinter\display2\components\Display */
            /* @var $image \pavlinter\display2\objects\Image */
            /* @var $originalImage \Imagine\Gd\Image */
            return $originalImage->thumbnail(new \Imagine\Image\Box($image->width, $image->height), \pavlinter\display2\objects\Image::MODE_OUTBOUND);
        }
    ],
  ],
],
```
Crop with url
-------------------
```php
//get all images from folder
//where 1 is id number from database
$images =  Yii::$app->display->getFileImgs(1, 'items', [
  'width' => 100,
  'height' => 100,
  'mode' => \pavlinter\display2\objects\Image::MODE_OUTBOUND,
  'loadingOptions' => [],
],[
  'dir' => 'mainDir',
  'minImages' => 2,
  'maxImages' => 6,
  'recursive' => false,
]);
//return from /display-images/items/1/mainDir/
[
  1204270244_1.jpg' => [
      'id_row' => 1
      'key' => 0
      'fullPath' => 'basePath..../web/display-images/items/1/1204270244_1.jpg'
      'dirName' => '1204270244_1.jpg'
      'imagesDir' => 'basePath..../web/display-images/items/1/'
      'imagesWebDir' => '/display-images/items/1/'
      'originImage' => '/display-images/items/1/1204270244_1.jpg'
      'image' => '1204270244_1.jpg'
      'display' => '/ru/display2/image/crop?width=100&height=100&mode=outbound&category=items&id_row=1&image=1204270244_1.jpg'
      'displayLoading' => '<div class=\"display\" style=\"width: 100px; height: 100px;\"><img src=\"/ru/display2/image/crop?width=100&amp;height=100&amp;mode=outbound&amp;category=items&amp;id_row=1&amp;image=1204270244_1.jpg\" alt=\"1204270244_1\"><div class=\"display-loading\"></div></div>'
  ]
]
echo Yii::$app->display->createUrl([
  'width' => 120,
  'image' => '/subfolders/bg.jpg',
  'category' => 'all',
  'mode' => \pavlinter\display2\objects\Image::MODE_OUTBOUND,
]);
echo Yii::$app->display->showImg([
  'id_row' => 2,
  'width' => 100,
  'image' => 'd.jpeg',
  'category' => 'items',
  'mode' => \pavlinter\display2\objects\Image::MODE_STATIC,
]);

echo Yii::$app->display->showImg([
  'id_row' => 2,
  'width' => 100,
  'image' => 'd.jpeg',
  'category' => 'items',
  'mode' => 'ownResizeMode',
]);
```

Crop now
-------------------

```php
echo Yii::$app->display->showCropImage([ //subfolders image
    'width' => 120,
    'image' => '/subfolders/bg.jpg', // or subfolders/bg.jpg
    'category' => 'all',
]);

echo Yii::$app->display->showCropImage([
    'id_row' => 2,
    'width' => 100,
    'image' => 'd.jpeg',
    'category' => 'items',
]);

echo Yii::$app->display->showCropImage([ //new name
    'name' => 'newName',
    'width' => 100,
    'height' => 130,
    'image' => '334.gif',
    'category' => 'all',
]);

echo Yii::$app->display->showCropImage([ //return default Html::img from items category
    'id_row' => 2,
    'width' => 100,
    'image' => 'rddddd',
    'category' => 'items',
]);
```