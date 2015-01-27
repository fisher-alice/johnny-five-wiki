<strike>Just write the damn docs.</strike>


When writing documentation be sure to at least include the following:
- [ ] Working annotated example code for all supported hardware
- [ ] A breadboard diagram to go with each code example
- [ ] Documentation for each class parameter

Annotated example code goes in the [eg/ folder](https://github.com/rwaldron/johnny-five/tree/master/eg), breadboard images are located in [docs/breadboard](https://github.com/rwaldron/johnny-five/tree/master/docs/breadboard) and the docs markdown files are generated using the grunt task: `grunt examples` [see task here](https://github.com/rwaldron/johnny-five/blob/master/Gruntfile.js#L209)