---
## create workspace


create app and lib sperately

add lib project into app, but not set copy if needed.

so lib changed , the lib mirror in app will changed.

---
## Xcode preferences

locations :

	set all default
	!not derived data, advance, custom path, relative to workspace
	
---
## add dependency

add lib target into compile dependency in [build phases]

add lib of the same workspace into library and framwork


---
## include *.h file

	add to [header search path] in [build setting]

	$(BUILT_PRODUCTS_DIR)/include/[libname]

	non-recurive


