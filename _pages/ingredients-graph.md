---
layout: default
permalink: /ingredients-graph/
---

<script src="path/to/sigma.js"></script>
<script src="path/to/sigma.parsers.gexf.min.js"></script>
<script>
  sigma.parsers.gexf(
    '/assets/skincare_backbone.gexf',
    { // Here is the ID of the DOM element that
      // will contain the graph:
      container: 'sigma-container'
    },
    function(s) {
      // This function will be executed when the
      // graph is displayed, with "s" the related
      // sigma instance.
    }
  );
</script>
