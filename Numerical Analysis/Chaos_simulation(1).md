

```julia
using PyPlot
```


```julia
function F(x,a)
    return a*x.*(1-x)
end
```




    F (generic function with 1 method)




```julia
function plotF(a)
    N=40
    x0=0.7
    
    xx=(a-1)/a
    x=ones(N+1,1)
    x[1]=x0
    for i=1:N
        x[i+1] = F(x[i],a)
    end
    
    ax=axes()
    ax[:set_ylim]([0,1])
    ax[:set_xlim]([0,N])
    plot(0:N,xx*ones(N+1,1))
    plot(0:N,x,marker="o")
    return "done"
end
    
```




    plotF (generic function with 1 method)




```julia
plotF(3.7)
```


![png](output_3_0.png)





    "done"




```julia
function plotF2(a)
    N=100
    x0=0.251
    
    x=ones(2N+1,1)
    y=ones(2N+1,1)
    x[1]=x0
    y[1]=0
    for i=1:N
        y[2i] = F(x[2i-1],a)
        x[2i]=x[2i-1]
        y[2i+1]=y[2i]
        x[2i+1]=y[2i]
    end
    
    ax=axes()
    ax[:set_ylim]([0,1])
    ax[:set_xlim]([0,1])
    plot(0:1,0:1)
    plot(0:0.01:1,F(0:0.01:1,a))
    plot(x,y)
    return x[100]
end
```




    plotF2 (generic function with 1 method)




```julia
plotF2(3.8)
```


![png](output_5_0.png)





    0.40477169045552247




```julia
function bifurcation()
    N=10000
    x0=0.5
    Na=400
    Nr=100
    b1=-2
    b2=-1.4
    
    Naa=(Na+1)*Nr
    xx=ones(Naa,1)
    yy=ones(Naa,1)
    
    for j=0:Na
        a=b1+(b2-b1)*j/Na
        x=ones(N+1,1)
        x[1]=x0
        for i=1:N
            x[i+1] = F(x[i],a)
        end
        xx[j*Nr+1:(j+1)*Nr] = x[N+2-Nr:N+1]
        yy[j*Nr+1:(j+1)*Nr] = a*ones(Nr,1)
    end
    
    ax=axes()
    ax[:set_ylim]([0,1])
    ax[:set_xlim]([b1,b2])
    plot(yy,xx,marker=".",linewidth=0)
    return "done"
end
```




    bifurcation (generic function with 1 method)




```julia
bifurcation()
```


![png](output_7_0.png)





    "done"




```julia
function gapF(a,e)
    N=100
    x0=0.5
    Ne=500
    
    xe=ones(Ne+1,1)
    for j=0:Ne
        x=ones(N+1,1)
        x[1]=x0+j*e
        for i=1:N
            x[i+1] = F(x[i],a)
        end
        xe[j+1]=x[N+1]
    end
    
    ax=axes()
    ax[:set_ylim]([0,1])
    ax[:set_xlim]([x0,x0+Ne*e])
    plot(x0+(0:Ne)*e,xe,marker="o")
    return "done"
end
```




    gapF (generic function with 1 method)




```julia
gapF(3.7,0.00001)
```


![png](output_9_0.png)





    "done"




```julia
function density(a,Ne)
    N=1000
    Ni=40
    x0=0.25
    e=0.5/Ne
    
    xe=ones(Ne+1,1)
    for j=0:Ne
        x=ones(N+1,1)
        x[1]=x0+j*e
        for i=1:N
            x[i+1] = F(x[i],a)
        end
        xe[j+1]=x[N+1]
    end
    xi=ones(Ni,1)
    for j=1:Ni
        xe.>(j-1)/Ni
        xe.<=j/Ni
        y=(xe.>(j-1)/Ni).*(xe.<=j/Ni)*1
        z=ones(1,Ne+1)*y
        xi[j]=z[1]
    end
    
    ax=axes()
    # ax[:set_ylim]([0,1])
    ax[:set_xlim]([0,1])
    plot((1:Ni)/Ni-1/2/Ni,xi,marker="o")
    return "done"
end
    
```




    density (generic function with 1 method)




```julia
density(3.77,1000)
```


![png](output_11_0.png)





    "done"


