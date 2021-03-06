The situation is that there are n chasers, and someone is observing them. At a given point of observation, any chaser can be
characterized by their position and speed at that point of observation. The speed is assumed to be constant and position varies with
every step the chaser runs. 

Problem is to determine that from a given observation point in time, at which each chaser is at a given position and running with a given speed, will all the chasers meet at some time in future at the same location? Means, after some time, will they beable to manage to gather at same position if they keep running with their corresponding speeds?

This is solved using two approaches, with the basic idea being that, 
suppose there are two chasers at t = 0, at positions x_1 and x_2, and running with speeds v_1 and v_2 respectively.
If they have to meet at some point, say after time t_f, and they meet at position t_f, then
total distance run by chaser 1 before meeting chaser 2 is x_f-x_1 and 
total distance run by chaser 2 before meeting chaser 1 is x_f-x_2.

Because they meet at time t_f, we know that
(x_f-x_1) = v_1*t
(x_f-x_2) = v_2*t

This is a set of two equations in two unknowns: x_f and t_f.
If x_f > 0 and t_f > 0, then we conclude that the chasers meet, otherwise they dont meet.

(B) Linear algebra [chasers_meet_linear_algebra.py]
For two chasers, it can be very easily solved. But for more number of chasers, we get more equations. Consider the case of four chasers.
They are running at speeds and positions given as x_1, v_1, x_2, v_2, x_3, v_3, x_4 and v_4. Their chase generates these four equations:
x_f-x_1 = v_1*t
x_f-x_2 = v_2*t
x_f-x_3 = v_3*t
x_f-x_4 = v_4*t

We can translate this to matrix multiplication 

--            --     -- --     -- --
|1  -v_1  0   0|     |x_f|     |x_1|
|1  -v_1  0   0|  *  |t_f|  =  |x_2| 
|1  -v_1  0   0|     |0  |     |x_3|
|1  -v_1  0   0|     |0  |     |x_4|
--            --     -- --     -- --
Notice the 0's are padded to the two matrices on LHS. This is because to solve x_f and t_f, we need to invert the 4x4 matrix and inversion needs the matrix to be square. Also, the second matrix on LHS needs to match its number of rows with number of columns in the first matrix. So two zeroes are padded into it. However, the 4x4 matrix created like this is singular and can not be inverted normally. We rather pseudo invert this 4x4 matrix and multiply it with the matrix on the RHS to solve x_f and x_t.

We can extend this idea to n chasers.

Lets look at the code:
See the example on line 51, as how the arguments are passed into the method. There are four parameters with two parameters corresponding to each of the chaser, means there are two chasers. 
On Line 13 we check if two parameters corresponding to each chaser is supplied and there are actually two or more chasers.
On line 21 we calculate the number of chasers.
On line 22-23 we prepare the zeroes for padding as explained their need above. We subtract 2 because two matrix elements (1 and -v_i) are already there.
On line 25-29 we segregate the position values and speed values into separate lists, towards construction of matrices.
On line 30-32 we convert the lists to numpy arrays so that matrix calculations can be made possible using numpy operators. We delete three lists which dont have any further use.
On line 33 we generate result by multiplying the pseudo inverse n x n matrix and column vector containing positions. The result vector contains the values of x_f and t_f that we need as a solution of this problem.
On line 35 we identify if x_f >= 0 and t_f >= 0 that makes real world sense, and publish our conclusion based on that.