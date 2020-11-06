<template>
  
</template>

<script>
export default {
    mounted(){
      window.requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame
        JTopo.Link.prototype.drawanimepic = function (imgurl, scene, width, height) {
        var imgnode = new JTopo.Node()

        imgnode.setSize(width || 16, height || 16)
        imgnode.setImage(imgurl)
        imgnode.zIndex = 2.5
        var thislink = this
        this.isremove = false
        function b (a, b) {
            var c = []
            if (a == null || b == null) return c
            if (a && b && a.outLinks && b.inLinks) {
            for (var d = 0; d < a.outLinks.length; d++) {
                for (var e = a.outLinks[d], f = 0; f < b.inLinks.length; f++) {
                var g = b.inLinks[f]
                e === g && c.push(g)
                }
            }
            }
            return c
        }
        function c (a, c) {
            var d = b(a, c)
            var e = b(c, a)
            var f = d.concat(e)
            return f
        }
        function d (a) {
            var b = c(a.nodeA, a.nodeZ)
            return b = b.filter(function (b) {
            return a !== b
            })
        }
        thislink.removeHandler = function () {
            this.isremove = true
            var a = this
            this.nodeA && this.nodeA.outLinks && (this.nodeA.outLinks = this.nodeA.outLinks.filter(function (b) {
            return b !== a
            })),
            this.nodeZ && this.nodeZ.inLinks && (this.nodeZ.inLinks = this.nodeZ.inLinks.filter(function (b) {
            return b !== a
            }))
            var b = d(this)
            b.forEach(function (a, b) {
            a.nodeIndex = b
            })
        }
        function imgnodeanime () {
            if (!thislink.isremove) {
            if (thislink.nodeA.outLinks) {
                var xs = thislink.nodeA.cx - thislink.nodeZ.cx
                var xy = thislink.nodeA.cy - thislink.nodeZ.cy
                var l = Math.floor(Math.sqrt(xs * xs + xy * xy))
                var j = l
                xl = xs / l, yl = xy / l
                var animespeed = (new Date() / 33)
                var colorpoint = parseInt(animespeed % l)
                imgnode.rotate = (Math.atan(xy / xs)) + (xs > 0 ? Math.PI : 0)
                imgnode.cx = thislink.nodeA.cx - colorpoint * xl
                imgnode.cy = thislink.nodeA.cy - colorpoint * yl
                window.requestAnimationFrame(imgnodeanime)
            }
            } else {
            scene.remove(imgnode)
            }
        }
        window.requestAnimationFrame(imgnodeanime)
        scene.add(imgnode)
        return imgnode
        }

    }
}
</script>

<style>

</style>