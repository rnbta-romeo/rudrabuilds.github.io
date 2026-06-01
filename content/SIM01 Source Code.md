```octave
clear;
clc;

%defining initial conditions
g = -9.81;
dt = 0.05;

h = 100; v = 0; t = 0;

%storage for plotting values
H_true = [];
H_meas = [];
T = [];

H_est = [];
A = [1 dt; 0 1];
B = [0; dt];
H = [1,0]
Q = [0.01 0; 0 0.01];
R = 4;
x_hat = [100; 0];
P = eye(2);


while h > 0
  H_true(end+1) = h;
  z = h + randn()*5;
  H_meas(end+1) = z;

  %prediction_step
  x_hat = A*x_hat + B*g;
  P = A*P*A'+ Q;

  %update_step
  y = z - H*x_hat;
  K = P*H'/(H*P*H'+R);
  x_hat = x_hat + K*y;
  P = (eye(2) - K*H)*P;

  T(end+1) = t;
  H_est(end+1) = x_hat(1);

  v = v + g*dt;
  h = h + v*dt;

  t = t + dt;
end;

plot(T, H_true, 'LineWidth', 1);
hold on;
plot(T, H_meas, '.-');
plot(T, H_est, 'LineWidth', 1);
grid on;

legend('True Height', 'Sensor Reading', 'Kalman Estimate');
xlabel('Time');
ylabel('Height');
```
