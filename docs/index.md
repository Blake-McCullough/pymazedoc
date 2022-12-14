# What is Pyamaze

The module pyamaze is created for the easy generation of random maze and apply different search algorithm efficiently. The main idea of this module, pyamaze, is to assist in creating customizable random mazes and be able to work on that, like applying the search algorithm with much ease.


## Mapping and traversing images
![Screenshot](img/pathstuff.png)
![Screenshot](img/mapdetails.png)
## Mapping and traversing code
```python
from pyamaze import maze,agent,COLOR
dictstore={}
maxy=5
maxz=5
def RCW():
    global direction
    k=list(direction.keys())
    v=list(direction.values())
    v_rotated=[v[-1]]+v[:-1]
    direction=dict(zip(k,v_rotated))

def RCCW():
    global direction
    k=list(direction.keys())
    v=list(direction.values())
    v_rotated=v[1:]+[v[0]]
    direction=dict(zip(k,v_rotated))

def moveForward(cell):
    if direction['forward']=='E':
        return (cell[0],cell[1]+1),'E'
    if direction['forward']=='W':
        return (cell[0],cell[1]-1),'W'
    if direction['forward']=='N':
        return (cell[0]-1,cell[1]),'N'
    if direction['forward']=='S':
        return (cell[0]+1,cell[1]),'S'

def wallFollower(m):
    global array
    global direction
    direction={'forward':'N','left':'W','back':'S','right':'E'}
    currCell=(m.rows,m.cols)
    path=''
    while True:

        print(currCell)
        if currCell in dictstore:
            pass
        else:
            dictstore[currCell]=m.maze_map[currCell]
        if len(dictstore) == (maxy*maxz):
            
            break
        if m.maze_map[currCell][direction['left']]==0:
            if m.maze_map[currCell][direction['forward']]==0:
                RCW()
            else:
                currCell,d=moveForward(currCell)
                path+=d
        else:
            RCCW()
            currCell,d=moveForward(currCell)
            path+=d
    print("DONE")
    print(path)
    print(len(m.maze_map))
    print(len(dictstore))
    print(dictstore)
    while True:




        if currCell==(1,1):
            break
        
        if dictstore[currCell][direction['left']]==0:
            if dictstore[currCell][direction['forward']]==0:
                RCW()
            else:
                currCell,d=moveForward(currCell)
                path+=d
        else:
            RCCW()
            currCell,d=moveForward(currCell)
            path+=d

    print(path)
    path2=path
    while 'EW' in path2 or 'WE' in path2 or 'NS' in path2 or 'SN' in path2:
        path2=path2.replace('EW','')
        path2=path2.replace('WE','')
        path2=path2.replace('NS','')
        path2=path2.replace('SN','')
    return path,path2
        



```
## Usage
```python
if __name__ == "__main__":
    m=maze(maxy,maxz)
    m.CreateMaze(loadMaze= r"E:\_TAFE_WORK\Robotics\MyMazeCrap\pyamaze\maze--2022-11-25--15-09-08.csv")
    #m.CreateMaze(saveMaze=True)
    b=agent(m,shape='arrow',color=COLOR.blue,footprints=True)
    c=agent(m,shape='square',color=COLOR.red,footprints=True)
    path,path2=wallFollower(m)

    assert (path == (1,1)) != True, "Cannot start there"
    m.tracePath({b:path2,c:path},delay=0)
    
    m.run()
```