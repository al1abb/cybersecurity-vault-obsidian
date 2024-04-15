<%* 
let description;
let includeDefaultUsage = false; // Set to false by default
let defaultUsage;
description = await tp.system.prompt("Enter Description:")
includeDefaultUsage = await tp.system.prompt("Should we include default usage? (1, 0)")
if(includeDefaultUsage == 1) {
	defaultUsage = await tp.system.prompt("Enter Default Usage:")
}
%> 
### `<%tp.file.title%>` — <% description %>

**Default Usage**
	`<% defaultUsage %>` 

**OPTIONS**
- A
- B
- C
