* 路径不能有中文名，不然mesh update的时候会报错
* cell zone condition 被自动认为是solid后（本来你的是流体），则需要双击点开修改属性
* 在SpaceClaim中生成几何后一定要fill，不然根本没有生成二维的模型，在mesh的时候就会报错
* 