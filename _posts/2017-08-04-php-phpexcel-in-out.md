---
layout: post
blog_id: "php-phpexcel-in-out"
title: "PHP利用phpExcel实现Excel数据的导入导出 "
date: 2017-08-04 00:00:00 -0700
tags: PHP
category: PHP
summary: 首先先说一下，这段例程是使用在Thinkphp的开发框架上，要是使用在其他框架也是同样的方法，很多人可能不能正确的实现Excel的导入导出，问题基本上都是phpExcel的核心类引用路径出错
comments: false
---

[phpExcel包的下载地址](http://download.csdn.net/detail/kesixin/9920920)

首先先说一下，这段例程是使用在Thinkphp的开发框架上，要是使用在其他框架也是同样的方法，很多人可能不能正确的实现Excel的导入导出，问题基本上都是phpExcel的核心类引用路径出错，如果有问题大家务必要对路径是否引用正确进行测试。

**从Excel表中导入数据**

1.前台html页面代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <form method="post" action="import" enctype="multipart/form-data">
        <h3>导入Excel表：</h3>
        <input  type="file" name="file-data" />
        <input type="submit"  value="导入" />
    </form>
</body>
</html>
```

2.将phpExcel包导入到ThinkPHP\Library\Vendor目录下。

3.在公共函数function.php中新建函数importExcel，代码如下：
```
/**
 *  读取excel表数据
 * @param string $filename 路径文件名
 * @param string $encode 返回数据的编码 默认为utf8
 * @return array $excelData 返回数组
 */
function importExcel($filename, $encode = 'utf-8')
{
    vendor("PHPExcel.PHPExcel");
    $objReader = PHPExcel_IOFactory::createReader('Excel5');
    $objReader->setReadDataOnly(true);
    $objPHPExcel = $objReader->load($filename);
    $objWorksheet = $objPHPExcel->getActiveSheet();

    $highestRow = $objWorksheet->getHighestRow();
    $highestColumn = $objWorksheet->getHighestColumn();
    $highestColumnIndex = PHPExcel_Cell::columnIndexFromString($highestColumn);
    
    $excelData = array();
    for ($row = 1; $row <= $highestRow; $row++) {
        for ($col = 0; $col < $highestColumnIndex; $col++) {
            $excelData[$row][] = (string)$objWorksheet->getCellByColumnAndRow($col, $row)->getValue();
        }
    }
    return $excelData;

}
```

4.在对应的php文件进行文件的处理：
```
/**
     * 导入excel表数据
     */
    public function import()
    {
        if (IS_POST) {
            if (!empty ($_FILES ['file-data'] ['name'])) {
                $tmp_file = $_FILES ['file-data'] ['tmp_name'];
                $file_types = explode(".", $_FILES ['file-data'] ['name']);
                $file_type = $file_types [count($file_types) - 1];

                /*判别是不是.xls文件，判别是不是excel文件*/
                if (strtolower($file_type) != "xls") {
                    $this->error('不是Excel文件，重新上传');
                }
                /*设置上传路径*/
                $savePath = './Public/';

                /*以时间来命名上传的文件*/
                $str = date('Ymdhis');
                $file_name = $str . "." . $file_type;

                /*是否上传成功*/
                if (!copy ( $_FILES['file-data']['tmp_name'], $savePath . $file_name )) {
                    echo "上传失败";
                }

                /*
                 对上传的Excel数据进行处理生成编程数据,这个函数会在下面第三步的importExcel中调用执行,
                  把Excel转化为数组并返回给$res,再进行数据库写入
                */
                $res = importExcel( $savePath . $file_name);
                var_dump($res);

                /*写入数据库*/
                ......
            }
        } else {
            $this->display();
        }
    }
```

**导出数据到Excel表中**

1.在公共函数function.php中新建函数exportExcel，代码如下：

```
/**
 * 导出数据
 * @param1 string $expTitle 文件名
 * @param2 array $expCellName  列名
 * @param3 array $expTableData 表数据
 */

function exportExcel($expTitle, $expCellName, $expTableData)
{
    $xlsTitle = iconv('utf-8', 'gb2312', $expTitle); //文件名称
    $cellNum = count($expCellName);
    $dataNum = count($expTableData);
    vendor("PHPExcel.PHPExcel");
    $objPHPExcel = new PHPExcel();
    //列的数目，可以根据需要添加
    $cellName = array('A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z',
        'AA', 'AB', 'AC', 'AD', 'AE', 'AF', 'AG', 'AH', 'AI', 'AJ', 'AK', 'AL', 'AM', 'AN', 'AO', 'AP', 'AQ', 'AR', 'AS', 'AT', 'AU', 'AV', 'AW', 'AX', 'AY', 'AZ');
    //设置字体样式
    $objPHPExcel->getActiveSheet()->getStyle('A1:AZ1')->getFont()->setName('黑体');
    $objPHPExcel->getActiveSheet()->getStyle('A1:AZ1')->getFont()->setSize(14);
//    $objPHPExcel->getActiveSheet()->getStyle('A1:AZ1')->getFont()->setBold(true);

    //$objPHPExcel->getActiveSheet(0)->mergeCells('A1:' . $cellName[$cellNum - 1] . '1'); //合并单元格
    //遍历生成表第一行->每一列的名称
    for ($i = 0; $i < $cellNum; $i++) {
        $objPHPExcel->setActiveSheetIndex(0)->setCellValue($cellName[$i] . '1', $expCellName[$i][1]);
    }
    //遍历填入数据
    for ($i = 0; $i < $dataNum; $i++) {
        for ($j = 0; $j < $cellNum; $j++) {
            $objPHPExcel->getActiveSheet(0)->setCellValue($cellName[$j] . ($i + 2), $expTableData[$i][$expCellName[$j][0]]);
        }
    }

    ob_end_clean(); //清空缓存
    header('Content-Type: application/vnd.ms-excel;charset=utf-8;name="' . $xlsTitle . '.xls"');
    header("Content-Disposition: attachment;filename=$xlsTitle.xls");
    header('Cache-Control: max-age=0');
    header('Cache-Control: max-age=1');
    header('Expires: Mon, 26 Jul 1997 05:00:00 GMT'); // Date in the past
    header('Last-Modified: ' . gmdate('D, d M Y H:i:s') . ' GMT'); // always modified
    header('Cache-Control: cache, must-revalidate'); // HTTP/1.1
    header('Pragma: public'); // HTTP/1.0
    $objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel5');
    $objWriter->save('php://output');
    exit;
}
```

2.在对应的php文件进行数据的查询：
```
    /**
     * 导出会员到excel表
     */
    function outExcel(){
        $xlsName="会员列表";
        $xlsCell=array(
            array('nickname','昵称'),
            array('realname','姓名'),
            array('idcard','身份证号'),
            array('phonenum','手机号码'),
            array('email','电子邮箱'),
            array('address','现居地址'),
            array('regtime','注册时间'),
        );
        $xlsData=D("user")->field('nickname,realname,idcard,phonenum,email,address,regtime')->select();

        exportExcel($xlsName, $xlsCell, $xlsData);
    }

}
```
以上就是PHP利用phpExcel实现Excel数据导入导出的全部内容。