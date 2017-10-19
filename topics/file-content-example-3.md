@title[Example 3]
## Inline SVG from Node Property
<span class="text-muted">Example 3</span>
```elm
prototype(WebExcess.Theme:Content) < prototype(Neos.Fusion:Tag) {
	@context.svgAsset = ${q(node).property('svg')}
	content = WebExcess.Theme:FileContent {
		resource = ${svgAsset.resource}
	}
}
```
@[3-5]
