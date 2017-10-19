## Loading asynchronous srcset
```elm
prototype(WebExcess.Theme:Asset) < prototype(Neos.Fusion:Tag) {
	@context.asset = ${q(node).property('asset')}
	tagName = 'img'
	attributes {
		src = ImageUri {
			asset = ${asset}
			allowCropping = true
			maximumWidth = 130
			maximumHeight = 174
		}
		data-srcset-pipe = ${'/_Asset/' + asset.identifier + '/srcset-c-260-347'}
		class = 'project-image'
	}
}
```

```html
<img
	src="/_Resources/Persistent/78cc314963bde8a4b806b69aa0f45357453d15f3/20170623_143018-130x73.jpg"
	data-srcset-pipe="/_Asset/101d9bb3-aef5-4531-a966-3a093c36c7e6/srcset-c-260-0"
	class="project-image">
```

+++

### The JavaScript (jQuery)
```javascript
$('[data-srcset-pipe]').not('.is-loading, .is-loaded').each((imageNode) => {
	imageNode.addClass('is-loading');

	try {
		let srcsetPipe = imageNode.attr('data-srcset-pipe');
		let xmlhttp = new XMLHttpRequest();

		xmlhttp.onreadystatechange = () => {
			if (xmlhttp.readyState == XMLHttpRequest.DONE ) {
				if (xmlhttp.status == 200) {
					let obj = JSON.parse(xmlhttp.responseText);
					imageNode.attr('srcset', obj.srcset);
					imageNode.addClass('is-loaded');
				}
			}
		};

		xmlhttp.open("GET", srcsetPipe, true);
		xmlhttp.send();
	} catch(error) {
		imageNode.removeClass('is-loading');
	}
});
```
@[5]
@[18]
@[12]

+++

### Routing
```yaml
-
  name: 'Asset Pipe Requests'
  uriPattern: '{node}_Asset/{assetidentifier}/srcset-{assetcropping}-{assetwidth}-{assetheight}'
  defaults:
    '@package': Neos.Neos
    '@controller': Frontend\Node
    '@action': show
    '@format': assetSmallPipe
  routeParts:
    node:
      handler: 'Neos\Neos\Routing\FrontendNodeRoutePartHandlerInterface'
  appendExceedingArguments: true
```
@[3]

+++

### Root.fusion
```elm
assetpipe = Neos.Fusion:Case {
	asset {
		condition = ${true}
		type = 'WebExcess.Theme:AssetPipeHandler'
	}
}
```
@[4]

+++

```elm
prototype(WebExcess.Theme:AssetPipeHandler) < prototype(Neos.Fusion:Http.Message) {
	httpResponseHead.headers.Access-Control-Allow-Origin = '*'
	httpResponseHead.headers.Content-Type = 'application/javascript;charset=utf-8'
	httpResponseHead.headers.Cache-Control = 'max-age=1209600'
	httpResponseHead.headers.Expires = ${Date.format(Date.now().timestamp + 1209600, 'D, d M Y H:i:s') + ' GMT'}

	content = Neos.Fusion:Array {
		small = WebExcess.Theme:AssetPipeHandler.ImageUri
		medium = WebExcess.Theme:AssetPipeHandler.ImageUri {
			size = 2
		}
		large = WebExcess.Theme:AssetPipeHandler.ImageUri {
			size = 4
		}

		@process.json = ${'{"srcset": ' + Json.stringify(value) + '}'}
	}
}
```
@[8]

+++

```elm
prototype(WebExcess.Theme:AssetPipeHandler.ImageUri) < prototype(ImageUri) {
	size = 1

	asset = ${Srcset.Pipe.getAsset(request.arguments.assetidentifier)}
	maximumWidth = ${request.arguments.assetwidth * size}
	maximumHeight = ${request.arguments.assetheight * size}
	maximumHeight.@if.isLargerThanZero = ${request.arguments.assetheight > 0 ? true : false}
	allowCropping = ${request.arguments.assetcropping == 'c' ? true : false}
	@process.size = ${value + ' ' + size + 'x'}
}
```
@[4]

+++

### Eel Helper
```php
/**
 * @Flow\Inject()
 * @var AssetRepository
 */
protected $assetRepository;

/**
 * @param $assetidentifier string
 * @return string
 */
public function getAsset($assetidentifier)
{
    /** @var \Neos\Media\Domain\Model\Image $asset */
    $asset = $this->assetRepository->findByIdentifier($assetidentifier);
    return $asset;
}
```

+++

```json
{
	"srcset": "
		/_Resources/Persistent/b0a8ab37a6ee770c6d532486977720ccbb3c3a5e/20170623_143018-260x146.jpg 1x,
		/_Resources/Persistent/5a1327b265d60360e8d8c21ff27a16eb2d487983/20170623_143018-520x293.jpg 2x,
		/_Resources/Persistent/5a1327b265d60360e8d8c21ff27a16eb2d487983/20170623_143018-1040x584.jpg 4x
	"
}
```

```html
<img
	src="/_Resources/Persistent/78cc314963bde8a4b806b69aa0f45357453d15f3/20170623_143018-130x73.jpg"
	data-srcset-pipe="/_Asset/101d9bb3-aef5-4531-a966-3a093c36c7e6/srcset-c-260-0"
	srcset="
		/_Resources/Persistent/b0a8ab37a6ee770c6d532486977720ccbb3c3a5e/20170623_143018-260x146.jpg 1x,
		/_Resources/Persistent/5a1327b265d60360e8d8c21ff27a16eb2d487983/20170623_143018-520x293.jpg 2x,
		/_Resources/Persistent/5a1327b265d60360e8d8c21ff27a16eb2d487983/20170623_143018-1040x584.jpg 4x
	"
	class="project-image">
```
