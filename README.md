# SmoothLineDrawing
A playground where I explore one way to smooth a path in SwiftUI
As promised, I'll talk about the nuttiness I was going through last week. I did solve it to my satisfaction, and it makes an excellent case study of how I think and how you might want to attack a problem. 

I got distracted last week because, once again, COVID demotivated me. RunDisney officially canceled the Disney Marathon and Princess Half Marathon races last week. I've wanted to get back into running but just haven't found either the time or motivation. My usual motivation is running one of those races or working out to an app I developed, miFitness trails, which follows a course on a map. 

Mifitness trails has a problem. While I lay out a path, it is jarring on how I take sharp corners. I wanted to make a better experience on the trail by smoothing out those sharp corners. I had no idea how to do smooth a curve. 

I did some research, but it became apparent quite quickly, I don't have the math skills anymore even to read the articles to make sense. 

That's when I fired up Swift Playgrounds on my iPad and started playing.  While I'm interested in Mapkit and Map annotations, I started smaller and went for simple lines.  I set up SwiftUI to make a path: 

`Path { path in
          path.move(to: CGPoint(x: pathArray[0].0, y: pathArray[0].1))
          for index in 1..<pathArray.count{
            let currentPoint = CGPoint(x: pathArray[index].0, y: pathArray[index].1)
            path.addLine(to: currentPoint)
            path.move(to: currentPoint)
          }
        }`

The code uses a global variable `pathArray` of type `[(Int,Int)]` for coordinates so I wouldnt have to take forever typing them in. I could set the coordinates of the line like this: 
`var pathArray:[(Int,Int)] = [(0,100),(200,100),(100,300),(200,300),(200,450),(0,450),(0,125)]`

This code produced a series of straight lines.  I added a second path to show the dots. 

The question is how to change the lines.  I did do some reading that seemed to indicated knowing the midpoints of other lines might help. 
So I wrote some code to get midpoints. Midpoints are the mean of coordinates, so Its relatively simpe. 
`func midPoint(start:CGPoint,end:CGPoint,weight:CGFloat = 2)->CGPoint{
  // find a midpoint
  let midX = (end.x + start.x)/weight
  let midY = (end.y + start.y)/weight
  return CGPoint(x: midX, y: midY)
} `

I'd add those points back into the list and use them somehow to smooth the curve. But looking only at code, I could not figure out how. 

That's when I wrote the newsletter last week. I wanted to show you an image of what I wanted to do. I  added the midpoints by hand.  This drawing process meant I had to think differently about the problem, and I came up with this algorithm: 

1. find midpoints before and after my target point. 
2. make a line with those two points
3. find the midpoint of that line. 
4. make a line from that new midpoint to my original point
5. find a midpoint from that. 
6. add the two points from step #1 and replace my original point with step  #5 to my path. 

I tried it, and it worked. 

You'll notice in the code one extra parameter: smoothing. I figured that if I used a different number instead of diving by two in the average for a midpoint, I'd get a different result. That is the case. So I added weight to this. This change creates other smoothing effects. At 1.5 instead of fo 2, I get this: 

It was a good experiment, and while I still haven't come up for the code in mapkMapKit, I did get the problem out of my head and know how to smooth lines at least one way. 

If you want to play with this and get all the code I worked on,you'll find the playground here. 

