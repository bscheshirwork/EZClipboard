EZClipboard
==========

This is a ZeroClipboard extension for the Yii Framework.

# Installing

* Download the EZClipboard zip file by clicking the ZIP button in the master repo
* Unzip the file and copy it to the /protected/extensions/ directory in your Yii app
* or add to you composer.json in same section
```
"repositories": [
	...
	{
		"type": "git",
		"url": "http://github.com/bscheshirwork/zero-clipboard"
	}
	...
],
"require": {
	...
	"bscheshir/zero-clipboard": "dev-master"
	...
}
```
and run update from your composer

# Usage
	Yii::import('vendor.bscheshir.zero-clipboard.EZClipboard', true);

	$this->widget('EZClipboard, array(
		'tag' => 'a',
		'tagHtmlOptions' => array('class'=>'copy-class'),
		'tagId' => 'copy_button',
		'clipboardText' => 'This is the text that will be copied'
	));
	
## Example

	<html>
		<head>
			<script type="text/javascript">
				function onLoad() {
					console.log('Movie Loaded');
				}

				function onComplete(client, args) {
	  				console.log("Copied text to clipboard: " + args.text );
				}
			</script>
		</head>
		<body>
			<? 
				Yii::import('vendor.bscheshir.zero-clipboard.EZClipboard', true);
				$this->widget('EZClipboard, array(
					'tagHtmlOptions' 	=> array('class'=>'copy-class'),
					'tagId' 			=> 'copy_button',
					'tagContent' 	 	=> "Copy Text",
					'clipboardText'		=> 'This is the text that will be copied',
					'zcEvents' 			=> array('load'=>'onLoad', 'complete'=>'onComplete'),
					'scriptPos'			=> 'HEAD'
				));
			?>			
		</body>
	</html>
	
Would produce the following code:

	<html>
		<head>
			<script type="text/javascript">
				function onLoad() {
					console.log('Movie Loaded');
				}

				function onComplete(client, args) {
	  				console.log("Copied text to clipboard: " + args.text );
				}
			</script>
			<script type="text/javascript" src="/assets/5fdfd183/js/ZeroClipboard.js"></script>
		</head>
		<body>
			<button class="copy-class" id="copy" data-clipboard-text="This is the text that will be copied">Copy Text</button>
			<script type="text/javascript">
				/*<![CDATA[*/
				jQuery(function($) {var clip = new ZeroClipboard($('#copy'), {"moviePath":"\/assets\/5fdfd183\/swf\/ZeroClipboard.swf"});clip.on('load', 'onLoad');clip.on('complete', 'onComplete');});
				/*]]>*/
				</script>	
		</body>
	</html>

## Usage in GridView with Ajax
```
Yii::import('vendor.bscheshir.zero-clipboard.EZClipboard', true);

$this->widget('CGridView', [
...
    'afterAjaxUpdate'                     => 'function(id, data) {
        $(".clipboard").each(function(index){
            new ZeroClipboard($(this), {"moviePath":EZClipboardMoviePath});
        });
    }',
...
    'columns'                             => [
...
        [
            'name'              => '',
            'type'              => 'raw',
            'value'             => function ($data, $row) {
                $this->widget('EZClipboard', [
                    'scriptPos'      => 'head',
                    'tag'            => 'i',
                    'tagHtmlOptions' => ['class' => 'clipboard icon-gift btn btn-mini btn-info'],
                    'tagContent'     => '',
                    'tagId'          => 'copy_button_' . $row,
                    'clipboardText'  => $data->getComment()
                ]);
            },
        ],
...
    ],
]);
```

## Options
  
**tag** - the type of tag to use (Default: 'button')  

	"tag" => "a"

**tagHtmlOptions** - the htmlOptions for the tag (i.e. 'id', 'class', etc.)  

	"tagHtmlOptions" => array(
		"class" => "copy_class"
	)

**tagContent** - the content between the tags    
	
	"tagContent" => "Copy Text"

**closeTag** - whether or not to use a closing tag  

	"closeTag" => false

**tagId** - shortcut for the tag ID, could also use tagHtmlOptions   

	"tagId" => "copy_button"

**zcOptions** - ZeroClipboard options 
	
	"zcOptions" => array('moviePath'=>'....')  
	
	see more http://xozblog.ru/2014/03/jquery-copy-clipboard/ 

**zcEvents** - ZeroClipboard events in an array 
	
	"zcEvents" => array('load'=>'onLoad')  

**clipboardText** - the text that will be copied when the user clicks the movie   

	"tagContent" => "This is the text that will be copied"

**scriptPos** - the position of the ZeroClipboard.js script tag (Default: 'END')  

	"scriptPos" => "HEAD"
