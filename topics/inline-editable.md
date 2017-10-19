## Inline Editable
```elm
prototype(WebExcess.Theme:InlineEditable) < prototype(Neos.Fusion:Tag) {
	property = null
	@context.property = ${this.property}

	content = ${q(node).property(property)}
	@process.contentElementEditable = ContentElementEditable {
		property = ${property}
	}
}
```

+++

### How to use it
```elm
prototype(WebExcess.Theme:Content) < prototype(Neos.Fusion:Tag) {
	content = WebExcess.Theme:InlineEditable {
		property = 'text'
	}
}
```
@[2-4]
