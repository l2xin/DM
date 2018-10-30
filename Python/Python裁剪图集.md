版权声明：转载请联系	https://blog.csdn.net/liyaxin2010/article/details/82796547

[使用shoebox]()
官网地址： http://renderhjs.net/shoebox/



Json格式
# -*- coding: utf-8 -*-

```python
import os,sys
import json
import os
import os.path
from PIL import Image
```

`

def json_to_dict(json_filename):
​    json_file = open(json_filename, 'r')
​    all_pic_dic = json.load(json_file)
​    all_item_list = []
​    for one_pic_item in all_pic_dic['res']:
​        one_json_item = all_pic_dic['res'][one_pic_item]
​        one_item = {}
​        one_item['name'] = one_pic_item.strip().lstrip().rstrip(',')
​        one_item['x'] = one_json_item['x']
​        one_item['y'] = one_json_item['y']
​        one_item['w'] = one_json_item['w']
​        one_item['h'] = one_json_item['h']
​        all_item_list.append(one_item)

    return all_item_list

   


def gen_png_from_json(folder_name, json_filename, png_filename):
​    big_image = Image.open(png_filename)
​    all_item_list = json_to_dict(json_filename)

    print 'gen_png_from_json:' + folder_name
    
    #清理掉原目录
    if not os.path.isdir(folder_name):
        #os.removedirs(folder_name)
        os.mkdir(folder_name)
    
    for i, one_item_data in enumerate(all_item_list):
        file_name = one_item_data['name']
        x = one_item_data['x']
        y = one_item_data['y']
        w = one_item_data['w']
        h = one_item_data['h']
    
        #设置图像裁剪区域 (x左上，y左上，x右下,y右下)
        image_box = [x, y, x + w , y + h ]
        one_pic = big_image.crop(image_box)
    
        one_pic.save(folder_name + "/" + file_name + '.png') # 存储裁剪得到的图像
        
        #print one_item_data

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
plist格式
# -*- co
#!python
import os,sys
from xml.etree import ElementTree
from PIL import Image

def tree_to_dict(tree):
​    d = {}
​    for index, item in enumerate(tree):
​        if item.tag == 'key':
​            if tree[index+1].tag == 'string':
​                d[item.text] = tree[index + 1].text
​            elif tree[index + 1].tag == 'true':
​                d[item.text] = True
​            elif tree[index + 1].tag == 'false':
​                d[item.text] = False
​            elif tree[index+1].tag == 'dict':
​                d[item.text] = tree_to_dict(tree[index+1])
​    return d

def gen_png_from_plist(plist_filename, png_filename):
​    file_path = plist_filename.replace('.plist', '')
​    big_image = Image.open(png_filename)
​    root = ElementTree.fromstring(open(plist_filename, 'r').read())
​    plist_dict = tree_to_dict(root[0])
​    to_list = lambda x: x.replace('{','').replace('}','').split(',')
​    for k,v in plist_dict['frames'].items():
​        rectlist = to_list(v['frame'])
​        width = int( rectlist[3] if v['rotated'] else rectlist[2] )
​        height = int( rectlist[2] if v['rotated'] else rectlist[3] )
​        box=( 
​            int(rectlist[0]),
​            int(rectlist[1]),
​            int(rectlist[0]) + width,
​            int(rectlist[1]) + height,
​            )
​        sizelist = [ int(x) for x in to_list(v['sourceSize'])]
​        rect_on_big = big_image.crop(box)

        if v['rotated']:
            rect_on_big = rect_on_big.rotate(90)
    
        result_image = Image.new('RGBA', sizelist, (0,0,0,0))
        if v['rotated']:
            result_box=(
                ( sizelist[0] - height )/2,
                ( sizelist[1] - width )/2,
                ( sizelist[0] + height )/2,
                ( sizelist[1] + width )/2
                )
        else:
            result_box=(
                ( sizelist[0] - width )/2,
                ( sizelist[1] - height )/2,
                ( sizelist[0] + width )/2,
                ( sizelist[1] + height )/2
                )
        result_image.paste(rect_on_big, result_box, mask=0)
    
        if not os.path.isdir(file_path):
            os.mkdir(file_path)
        outfile = (file_path+'/' + k).replace('gift_', '')
        print outfile, "generated"
        result_image.save(outfile)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
注：依赖PIL,本文使用[Python Imaging Library 1.1.7 for Python 2.7]
官网 : http://www.pythonware.com/products/pil/
安装完后执行

		cd C:\Python27\scripts\
		pip install pillow
1
2
完整示例: https://github.com/l2xin/UnpackSpriteSheet

Github：https://github.com/l2xin
qq群：215974591，欢迎加群共同探讨学习。
个人微信公众号：牵蜗牛看世界

---------------------

