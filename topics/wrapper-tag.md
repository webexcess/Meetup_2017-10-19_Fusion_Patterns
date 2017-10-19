## Wrapper Tag
```elm
prototype(WebExcess.Theme:Helper.WrapTag) < prototype(Neos.Fusion:Tag) {
	content = ${value}
}
```

+++

### How to use it
@[4-6]
```elm
prototype(WebExcess.Theme:Column) < prototype(Neos.Fusion:Tag) {
	attributes.class = 'content-element'
	content = Neos.Fusion:Array {
		@process.wrapTag = WebExcess.Theme:Helper.WrapTag {
			attributes.class = 'row'
		}

		left = Neos.Fusion:Tag {
			attributes.class = 'col-sm-6'
			content = 'Left content'
		}

		right = Neos.Fusion:Tag {
			attributes.class = 'col-sm-6'
			content = 'Right content'
		}
	}
}
```

+++

### Output
```html
<div class="content-element">
	<div class="row">
		<div class="col-sm-6">Left content</div>
		<div class="col-sm-6">Right content</div>
	</div>
</div>
```
