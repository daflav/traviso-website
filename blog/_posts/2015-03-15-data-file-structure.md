---
layout: post
title: "Data File Structure"
date:   2015-03-15 11:00:00
summary: A detailed explanation of the data file's structure.
---

___

Data file is a simple XML file which defines what goes where inside the engine.

<!--more-->

Here is an example:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<map_data>
	<tiles>
		<tile id="1" movable="1">t_1.png</tile>
	</tiles>
	<objects>
	    <object id="1" movable="0" interactive="0" s="1x1">
			<v id="idle">
				<f>o_1.png</f>
			</v>
		</object>
	</objects>
	<ground_map>
		<row>1 ,1 ,1 ,1</row>
		<row>1 ,0 ,0 ,1</row>
		<row>1 ,0 ,0 ,1</row>
		<row>1 ,1 ,1 ,1</row>
	</ground_map>
	<object_map>
		<row>0 ,0 ,0 ,0</row>
		<row>0 ,1 ,0 ,0</row>
		<row>0 ,0 ,1 ,0</row>
		<row>0 ,0 ,0 ,0</row>
	</object_map>
<map_data>
```

So let's go step by step.

<br/>
### Tiles

`<tiles>` tag defines the available tiles that you can use in your map.

```xml
<tiles>
	<tile id="1" movable="1">images/grassTile.png</tile>
	<tile id="2" movable="0">images/waterTile.png</tile>
</tiles>
```

A `<tile>` tag should include the following attributes:

* **id:** (Numbers except 0) This is the key value that you will use to define the type of the tile. These will be used in the `<ground_map>` tag. DON'T use 0 as id since it represents empty tiles in the `<ground_map>`.

* **movable:** (0/1) This defines weather other characters can move onto this type of tile or not.
            
A `<tile>` tag should also include the image path/name as the tag value.

<br/>
### Objects

Similarly `<objects>` tag defines the available objects that you can use in your map.

```xml
<objects>
    <object id="1" movable="0" interactive="0" s="1x1">
		<v id="idle">
			<f>boxes.png</f>
		</v>
	</object>
</objects>
```

An `<object>` tag should include the following attributes:

* **id:** (Numbers except 0) This is the key value that you will use to define the type of the object. These will be used in the `<object_map>` tag. DON'T use 0 as id since it represents no-objects in the `<object_map>`.

* **movable:** (0/1) This defines weather other characters can move onto a tile that this objects sits on.

* **interactive:** (0/1) This specifies weather the user can select/interact with the object. The engine will use the callbacks to inform the developer once the object is selected by the user.

* **s:** This specifies the size of the object in the number of rows and colums (rows x colums).

An `<object>` tag should also include AT LEAST one `<v>` tag with `id="idle"`.
            
> **NOTE:** You can define as many visuals (single images or animation sequences) as you want using `<v>` tags.

Here is an animation defined for the idle state of an object. You can think of `<f>` tag as a frame for animation. If there is only one `<f>` tag then it is a still image instead of an animation.

```xml
<v id="idle">
    <f>hero_stand_1.png</f>
    <f>hero_stand_2.png</f>
    <f>hero_stand_3.png</f>
    <f>hero_stand_4.png</f>
    <f>hero_stand_5.png</f>
    <f>hero_stand_6.png</f>
</v>
```

You can create the textures/animations for your object in two ways.

* Either using child `<f>` tags to specify each frame texture if your image names are not in a numeric order like `walk1.png, walk2.png ...`
  
```xml
<v id="move_ne">
    <f>hero_move_ne_x.png</f>
    <f>hero_move_ne_w.png</f>
    <f>hero_move_ne_y.png</f>
    <f>hero_move_ne_z.png</f>
    <f>hero_move_ne_t.png</f>
    <f>hero_move_ne_v.png</f>
</v>
```
* Or in a single `<v>` tag by providing filename/path prefix and extension.

```xml  
<v id="move_ne" path="hero_move_ne_" ext="png" number_of_frames="6" start_number="1" /> 
```

<br/>
So, the following two `<v>` tags are interpreted as the same by the engine:
 
```xml
<v id="flip">
    <f>hero_flip_3.png</f>
    <f>hero_flip_4.png</f>
    <f>hero_flip_5.png</f>
    <f>hero_flip_6.png</f>
</v>
 
<v id="flip" path="hero_flip_" ext="png" number_of_frames="4" start_number="3" /> 
```

Here the attributes go as follows:

* **id:** Id of the texture or animation. You will use this to change object texture/animation any time.

* **path:** Image name/id as it is accessed throughout a cached spritesheet or with a path externally.

* **ext:** Image file extension. i.e. png, jpg

* **number_of_frames:** Number of frames in an animation. if it is not an animation but a single texture, set it to 1.

* **start_number:** Starting number of the image name suffix. This is only used for animations where image names are in a numeric order like walk1.png, walk2.png, walk3.png, walk4.png ...
                
> **NOTE 1:** Following ids are reserved, so for additional custom animations use different ids:

> `idle_s, idle_sw, idle_w, idle_nw, idle_n, idle_ne, idle_e, idle_se, move_s, move_sw, move_w, move_nw, move_n, move_ne, move_e, move_se`
	
> **NOTE 2:** `<f>` tags of a `<v>` tag has priority over attributes defining animation sequences. So if you use both the engine will process only the `<f>` tags.

> **NOTE 3:** For an object, at minimum, one `<v>` tag with the `id="idle"` is required. This will also be used as the initial visual (single texture or animation sequence) that will be applied to your object until you change it in your own logic.

<br/>
### Ground Map


<br/>
### Object Map



<a href="https://github.com/axaq/traviso.js" target="_blank">Download</a> traviso and start playing around with the examples included.

Check out the documentation <a href="http://www.travisojs.com/docs/" target="_blank">here</a>.

<div id="post-navigation" >
  <div class="previous">
    {% if page.previous.url %}
    <a href="{{page.previous.url}}" title="Previous post: {{page.next.title}}">
      <i class="fa fa-lg fa-arrow-circle-left"></i>
      {{page.previous.title}}
    </a>
    {% endif %}
  </div>
  <div class="next text-right">
    {% if page.next.url %}
    <a href="{{page.next.url}}" title="Next post: {{page.next.title}}">
    	NEXT: {{page.next.title}}
    	<i class="fa fa-2x fa-arrow-circle-right"></i>
    </a>
    {% endif %}
  </div>
</div>

