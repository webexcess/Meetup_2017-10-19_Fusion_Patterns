@title[Example 2]
## Inline SVG from Static File
<span class="text-muted">Example 2</span>
```elm
prototype(WebExcess.Theme:Header.Logo) < prototype(Neos.Fusion:Tag) {
	attributes.class = 'logo'
	content = WebExcess.Theme:FileContent {
		path = 'resource://WebExcess.Theme/Public/Assets/Logo.svg'
	}
}
```
@[3-5]
