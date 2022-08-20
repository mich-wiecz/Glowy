<template>
  <canvas ref="canvas"  :width="width" :height="height" />
</template>

<script>
export default {
  props: {
    width: {
      type: Number,
      default: 600
    },
    branches: {
      type: Number,
      default: 2
    },
    color: {
      type: String,
      default: 'gray'
    },
    creationRate: {
      type: Number,
      default: .8
    },
    animated: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    triangleSide() {
      return this.width / 1.5
    },
    height() {
      return this.getTriangleHeight(this.triangleSide) * 2
    }
  },
  methods: {
      getTriangleHeight(sideLength) {
        return (Math.sqrt(3) / 2) * sideLength
      },
      drawTriangle(ctx, points) {
        const [pointA, pointB, pointC] = points

        ctx.moveTo(pointA.x, pointA.y)
        ctx.lineTo(pointB.x, pointB.y)
        ctx.stroke()
        ctx.lineTo(pointC.x, pointC.y)
        ctx.stroke()
        ctx.lineTo(pointA.x, pointA.y)
        ctx.stroke()
      },
      drawTriangleTimely(ctx, points, divisions = 100) {
        const [pointA, pointB, pointC] = points

        let current = 0

        function getNextStep(pointA, pointB) {

          const ratio = current / divisions
          const result = {}

          if (pointB.x >= pointA.x) {
            result.x = pointA.x + (pointB.x - pointA.x) * ratio
          } else {
            result.x = pointA.x - (pointA.x - pointB.x) * ratio
          }

          if (pointA.y >= pointB.y) {
            result.y = pointA.y - (pointA.y - pointB.y) * ratio
          } else {
            result.y = pointA.y + (pointB.y - pointA.y) * ratio
          }

          return [result.x, result.y]
        }

        function drawPart() {
          ctx.moveTo(pointA.x, pointA.y)
          ctx.lineTo(...getNextStep(pointA, pointB))
    
          ctx.moveTo(pointB.x, pointB.y)
          ctx.lineTo(...getNextStep(pointB, pointC))
    
          ctx.moveTo(pointC.x, pointC.y)
          ctx.lineTo(...getNextStep(pointC, pointA))
    
          ctx.stroke()
          current++

          if (current <= divisions) {
            requestAnimationFrame(drawPart)
          }
        }

        requestAnimationFrame(drawPart)

      },
      getChildTrianglePoints(parentPoints) {
        const [pointA, pointB, pointC] = parentPoints


        function getLineLength(pointA, pointB) {
          return Math.sqrt((pointB.x - pointA.x)**2 + (pointB.y - pointA.y)**2)
        }

        const lineLength = getLineLength(pointA, pointB)

        const lineRelativeHeight = pointA.y - pointB.y
        const lineRelativeWidth = pointA.x - pointB.x
        let lineHeight = Math.abs(lineRelativeHeight)
        let lineWidth = Math.abs(lineRelativeWidth)
        if (lineHeight < 1) {
          lineHeight = 0
        }
        if (lineWidth < 1) {
          lineWidth = 0
        }


        let childABHeightVector
        if (lineRelativeHeight < 0) {
          childABHeightVector = 1
        } else if (lineRelativeHeight === 0) {
          childABHeightVector = 0
        } else {
          childABHeightVector = -1
        }

        let childABWidthVector
        if (lineRelativeWidth < 0) {
          childABWidthVector = 1
        } else if (lineRelativeWidth === 0) {
          childABWidthVector = 0
        } else {
          childABWidthVector = -1
        }
        

        let childA = {
          x: pointA.x + (lineWidth / 4) * childABWidthVector,
          y: pointA.y + (lineHeight / 4) * childABHeightVector
        }
        let childB = {
          x: pointA.x + (lineWidth * (3/4)) * childABWidthVector,
          y: pointA.y + (lineHeight * (3/4)) * childABHeightVector
        }

        const childSideLength = getLineLength(childA, childB)
        
        let skewXRatio = lineWidth === 0 ? 1 : lineLength / lineWidth
        if (lineHeight === 0) {
          skewXRatio = 2
        }
        let skewYRatio = lineHeight === 0 ? 1 : lineLength / lineHeight
        if (lineWidth === 0) {
          skewYRatio = 2
        }

        const childCenter = {
          x: pointA.x + (lineWidth / 2) * childABWidthVector,
          y: pointA.y + (lineHeight / 2) * childABHeightVector
        }

        const childCWidthVector = pointC.x - childCenter.x <= 0 ? 1 : -1
        const childCHeightVector = pointC.y - childCenter.y <= 0 ? 1 : -1


        let childCXMove = (childSideLength / skewXRatio) * childCWidthVector
        if (lineWidth === 0) {
          childCXMove = this.getTriangleHeight(childSideLength) * childCWidthVector
        }

        let childCYMove =  (childSideLength / skewYRatio) * childCHeightVector
        if (lineHeight === 0) {
          childCYMove = this.getTriangleHeight(childSideLength) * childCHeightVector
        }

        
        const childC = {
          x: childCenter.x + childCXMove * (lineHeight === 0 ? 0 : 1.5),
          y: childCenter.y + childCYMove / (lineHeight === 0 ? 1 : 2)
        }

        return [childA, childB, childC]

      },
      getPointsCombinations(points, isFirst) {
        let [pointA, pointB, pointC] = points

      const combinations = [
        [pointA, pointC, pointB],
        [pointC, pointB, pointA]
      ]

      if (isFirst) {
        combinations.push([pointA, pointB, pointC])
      }

      return combinations
      },
      drawSnowflake(canvas, noOfChildren) {

        const ctx = canvas.getContext('2d')
    
        const canvasWidth = ctx.canvas.width
    
        const mainTriangleSide = this.triangleSide

        const mainTriangleHeight = this.getTriangleHeight(mainTriangleSide)
    
        const xStart = (canvasWidth - mainTriangleSide) / 2
        const yStart = 1.25 * mainTriangleHeight
    
        ctx.fillStyle = '#000000'
        ctx.strokeStyle = this.color
        ctx.beginPath();

        const mainTriangleA = {x: xStart, y: yStart}
        const mainTriangleB = {x: xStart + mainTriangleSide, y: yStart}
        const mainTriangleC = {x: xStart + mainTriangleSide / 2, y: yStart - mainTriangleHeight}

        this.drawTriangle(ctx, [mainTriangleA, mainTriangleB, mainTriangleC])

        const maxChildren = noOfChildren

        const drawChildren = (parentPoints, noOfChildren) => {
          
          if (noOfChildren === 0) {
            return
          }

          const childCombinations = this.getPointsCombinations(parentPoints, noOfChildren === maxChildren)

          childCombinations.forEach(combination => {
            const childPoints = this.getChildTrianglePoints(combination)
            if (this.animated) {
              this.drawTriangleTimely(ctx, childPoints, 130 * this.creationRate)
            } else {
              this.drawTriangle(ctx, childPoints)
            }
           

            drawChildren(childPoints, noOfChildren - 1)
          })

        }

        drawChildren([mainTriangleA, mainTriangleB, mainTriangleC], noOfChildren)
    
      }
  },
  watch: {
    branches() {
      this.drawSnowflake(this.$refs.canvas, this.branches, this.color)
    }
  },
  updated() {
     const canvas = this.$refs.canvas
    this.drawSnowflake(canvas, this.branches, this.color)
  },
  mounted() {
      const canvas = this.$refs.canvas
      this.drawSnowflake(canvas, this.branches, this.color)
  }
}
</script>

<style>

</style>