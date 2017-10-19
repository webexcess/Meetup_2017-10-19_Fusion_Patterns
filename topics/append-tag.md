## Append Tag
```elm
prototype(WebExcess.Theme:Helper.BreakTag) < prototype(Neos.Fusion:Value) {
	value = Neos.Fusion:Array {
		value = ${value}
		breakTag = Neos.Fusion:Tag {
			tagName = 'br'
			selfClosingTag = true
		}
	}
}
```

+++

### How to use it
@[6]
```elm
prototype(WebExcess.Theme:Address) < prototype(Neos.Fusion:Tag) {
	tagName = 'address'
	content = Neos.Fusion:Array {
		title = Neos.Fusion:Value {
			value = 'webexcess GmbH'
			@process.break = WebExcess.Theme:Helper.BreakTag
		}

		address = Neos.Fusion:Value {
			value = 'Grubenstrasse 12'
			@process.break = WebExcess.Theme:Helper.BreakTag
		}

		city = Neos.Fusion:Value {
			value = '8045 Zürich'
		}
	}
}
```

+++

### Output
```html
<address>
	webexcess GmbH<br />Grubenstrasse 12<br />8045 Zürich
</address>
```
