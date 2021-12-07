<template>
  <div class="counter">
    <textarea v-model="message" placeholder="add multiple lines"></textarea>
    <br />
    <button @click="updateData">Update</button>
    <input type="file" ref="doc" @change="readFile" />
    <br/>
    <span>Output:</span>
    <p style="white-space: pre-line;">{{ output }}</p>
    
  </div>
</template>

<script>

export default {
  name: 'testComponent2',
  data: function() {
    return {
      message: "",
      output: ""
    }
  },
  methods:
  {
    readFile: function () {
      console.log("read")
      this.file = this.$refs.doc.files[0];
      const reader = new FileReader();
      if (this.file.name.includes(".txt")) {
        reader.onload = (res) => {
          this.message = res.target.result;
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(this.file);
      } else {
        reader.onload = (res) => {
          this.message(res.target.result);
        };
        reader.onerror = (err) => console.log(err);
        reader.readAsText(this.file);
      }
    },

    updateData: function () {
      if(!this.message)
        this.readFile();
      
     var ventLines = this.message.split('\n').reduce(findCoordinates,null);
    var ventZone = ventLines.reduce(mapZone, null)
    console.log("result " + ventZone.count)
    
 
    }
  }
}

function mapZone(zone, current)
{
  current.startCoord[0] = parseInt(current.startCoord[0]);
  current.startCoord[1] =parseInt(current.startCoord[1]);
  current.endCoord[0] = parseInt(current.endCoord[0]);
  current.endCoord[1] = parseInt(current.endCoord[1]);
  console.log("mapping ")
  console.log(current.startCoord)
  if(!zone)
  {
    zone = 
    {
      grid: [[]],
      count: 0
    }
  }
  if(current.startCoord[0] === current.endCoord[0])
  {
    var start = current.startCoord[1] < current.endCoord[1] ? current.startCoord[1] : current.endCoord[1]
    var end = current.startCoord[1] > current.endCoord[1] ? current.startCoord[1] : current.endCoord[1]
    for (let index = start; index <= end; index++) {
      if(!zone.grid[current.startCoord[0]])
        zone.grid[current.startCoord[0]]=[];
       if(!zone.grid[current.startCoord[0]][index])
        zone.grid[current.startCoord[0]][index]=0;
     zone.grid[current.startCoord[0]][index]++;
     if(zone.grid[current.startCoord[0]][index]==2)
      zone.count++;
    }
  }
  else if(current.startCoord[1] === current.endCoord[1])
  {
    start = current.startCoord[0] < current.endCoord[0] ? current.startCoord[0] : current.endCoord[0]
    end = current.startCoord[0] > current.endCoord[0] ? current.startCoord[0] : current.endCoord[0]
    for (let index = start; index <= end; index++) {
      if(!zone.grid[index])
        zone.grid[index]=[];
      if(!zone.grid[index][current.startCoord[1]])
        zone.grid[index][current.startCoord[1]]=0;
      zone.grid[index][current.startCoord[1]]++;
     if(zone.grid[index][current.startCoord[1]]==2)
      zone.count++;
      //console.log("x " + index + " y " + current.startCoord[1] + " count" +zone.grid[index][current.startCoord[1]])
    }
  }
  else{
    var yOffset = 0;
    var slope = current.startCoord[1] < current.endCoord[1] ? 1 : -1
    start = current.startCoord[0] < current.endCoord[0] ? current.startCoord[0] : current.endCoord[0]
    end = current.startCoord[0] > current.endCoord[0] ? current.startCoord[0] : current.endCoord[0]
    var yBase = start === current.startCoord[0] ? current.startCoord[1] : current.endCoord[1]
    slope *= start === current.startCoord[0] ? 1 : -1
    for (let index = start; index <= end; index++) {
      var yIndex = yBase+yOffset;
      if(!zone.grid[index])
        zone.grid[index]=[];
      if(!zone.grid[index][yIndex])
        zone.grid[index][yIndex]=0;
      zone.grid[index][yIndex]++;
      if(zone.grid[index][yIndex]==2)
        zone.count++;
      // console.log("x " + index + " y " + yIndex + " count" +zone.grid[index][yIndex] + " slope " + slope + " yoffset " + yOffset)
      yOffset += slope;
      
    }
  }
  console.log(current)
  console.log(zone)
  return zone;
}

function findCoordinates(list, current)
{
  if(!list)
    list = new Array();
  var ventLine = {}
  var coordStrings = current.split(" -> ")
  ventLine.startCoord = coordStrings[0].split(',')
  ventLine.endCoord = coordStrings[1].split(',')
  console.log(list);
  list.push(ventLine);
  return list;
}

</script>

