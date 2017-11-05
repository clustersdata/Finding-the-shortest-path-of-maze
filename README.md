# Finding-the-shortest-path-of-maze

Finding the shortest path of maze

	（广度优先搜索算法）
  
【参考程序】

uses crt;

const

migong:array  [1..5,1..5] of integer=((0,0,-1,0,0), (0,-1,0,0,-1),

			(0,0,0,0,0), (0,-1,0,0,0),  (-1,0,0,-1,0));
      
			{迷宫数组}
      
fangxiang:array  [1..4,1..2] of -1..1=((1,0),(0,1),(-1,0),(0,-1));

					{方向增量数组}
          
type node=record

	     lastx:integer;  {上一位置坐标}
       
	     lasty:integer;
       
	     nowx:integer;   {当前位置坐标}
       
	     nowy:integer;
       
	     pre:byte;	    {本结点由哪一步扩展而来}
       
	     dep:byte;	    {本结点是走到第几步产生的}
       
	  end;
    
var


    lujing:array[1..25] of node;   {记录走法数组}
    
    closed,open,x,y,r:integer;
    
procedure output;

var i,j:integer;

begin

  for i:=1 to 5 do  begin
  
    for j:=1 to 5 do
    
      write(migong[i,j]:4); writeln;end;
      
  i:=open;
  
  repeat
  
     with lujing[i] do
     
	 write(nowy:2,',',nowx:2,' <--');
   
     i:=lujing[i].pre;
     
  until lujing[i].pre=0;
  
  
     with lujing[i] do
     
	 write(nowy:2,',',nowx:2);
   
end;

begin

  clrscr;
  
  with lujing[1] do begin  {初始化第一步}
  
     lastx:=0;	lasty:=0; nowx:=1;nowy:=1;pre:=0;dep:=1;end;
     
  closed:=0;open:=1;migong[1,1]:=1;
  
  repeat
 
    inc(closed); {队列首指针加1,取下一结点
    
    }
    
    for r:=1 to 4 do begin	{以4个方向扩展当前结点}
    
      x:=lujing[closed].nowx+fangxiang[r,1]; {扩展形成新的坐标值}
      
      y:=lujing[closed].nowy+fangxiang[r,2];
      
      if not ((x>5)or(y>5) or (x<1) or (y<1) or (migong[y,x]<>0)) then begin
      
					{未出界,未走过则可视为新的结点}
          
	      inc(open);   {队列尾指针加1}
        
	      with lujing[open] do begin  {记录新的结点数据}
        
		nowx:=x; nowy:=y;
    
		lastx:=lujing[closed].nowx;{新结点由哪个坐标扩展而来}
    
		lasty:=lujing[closed].nowy;
    
		dep:=lujing[closed].dep+1; {新结点走到第几步}
    
		pre:=closed;		   {新结点由哪个结点扩展而来}
    
	      end;
        
	      migong[y,x]:=lujing[closed].dep+1;  {当前结点的覆盖范围}
        
	      if (x=5) and (y=5) then begin  {输出找到的第一种方案}
        
			  writeln('ok,thats all right');output;halt;end;
        
      end;
      
    end;
    
  until closed>=open; {直到首指针大于等于尾指针,即所有结点已扩展完}
  
end.

