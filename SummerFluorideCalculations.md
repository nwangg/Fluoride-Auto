#Calculations for Water Pump speed
Calculates RPM of water pump, given a desired upflow velocity in the sedimentation tube.

```python
import math as m
import numpy as np
from aide_design.play import*
import aide_design.floc_model as fm

#Q=.76*(u.milliliter)/(u.second)
#Q.to(u.m*u.m*u.m/u.s)
D=(1)*u.inch
D=D.to(u.m)
A=np.pi*(D**2)/4
print(A)
v=(0.0015)*(u.m/u.s)
#Area given our diameter of tubing
Q=(A*v)
print(Q)
Q=Q.to((u.milliliter)/(u.second))
print(Q)
#Manual calculation of RPM
Man_RPM=(Q/((0.8)*(u.milliliter)))
Man_RPM=Man_RPM*(60*(u.second))
print(Man_RPM)

```

#Calculations for Coagulant Pump speed
Calculates RPM of coagulant pump, given a desired concentration in the system.
Assumes flow rate of coagulant pump is negligible compared to the water pump flow rate.

```python
#Aim for 6.5 mg/L of PAC the lowest according to Github Issues
```

#Fluoride pump speed and stock concentration
Calculates volumetric flow rate of fluoride pump, given pump speed in RPM.
Calculates concentration of fluoride in system.
Assumes flow rate of fluoride pump is negligible compared to the water pump flow rate.
Uses tube sizing conversions found on [AguaClara Confluence ](https://confluence.cornell.edu/display/AGUACLARA/Auto+Tutorial+for+Peristaltic+Pumps).

```python

#Assume Qstock, Qsystem and Csystem

pump_speed = 5*(u.rpm)
#yellow_blue = 0.149*(u.milliliter/u.revolutions)
#yb_flowrate = yellow_blue.to(u.liter/u.revolutions)*(pump_speed).to(u.revolutions/u.s)
orange_yellow = 0.019*(u.milliliter/u.revolutions)
oy_flowrate = orange_yellow.to(u.liter/u.revolutions)*(pump_speed).to(u.revolutions/u.s)

print('The fluoride flow rate is: '+str(oy_flowrate)) #Qstock

Q_sys=Q.to((u.liter)/(u.second)) #From Calculations for Water Pump speed and assume oy_flowrate is negligible for now
Q_stock = oy_flowrate
C_sys = 5*(u.mg/u.L) #user input desired concentration of F- in the system

C_stock= (Q_sys*C_sys)/Q_stock
print('The fluoride concentration in the stock is: ' +str(C_stock))
#M1V1=M2V2 to obtain volume of fluoride stock needed
M_superstock = 10000 * (u.mg/u.L) #concentration of fluoride provided
M_stock = C_stock
V_stock = 0.5 * u.L #total volume of the stock (water+fluoride)
V_superstock= (M_stock*V_stock)/M_superstock
print('The fluoride volume in the stock is: ' +str(V_superstock))
V_water=V_stock-V_superstock
print('The water volume in the stock is : ' +str(V_water))

#v_stock = (675 * u.mL).to(u.L)
#v_water = 1 * u.L
#total_v = v_water + v_stock
#c_solution = (fluoride_stock * v_stock)/(total_v)
#print('The fluoride concentration in the stock container is: ' +str(c_solution))

#Q_sys = Q.to(u.L/u.s) #water pump speed, calculated above
#Q_stock = yb_flowrate

#C_stock = c_solution
#C_sys = ((Q_stock*C_stock)/(Q_sys)).to(u.mg/u.L)
#print('The fluoride concentration in the system is: ' +str(C_sys))

#percent_flow = (yb_flowrate/Q_sys)*100
percent_flow = (oy_flowrate/Q_sys)*100
print('The percent flow rate of fluoride/total flow through system: '+str(percent_flow)+' %.')

```