## Building/Running
- Install the current package: `go install`
- Add missing modules and remove unused: `go mod tidy`

## Error Handling
- use x.Must(function) to panic if function returngs a non nil error
