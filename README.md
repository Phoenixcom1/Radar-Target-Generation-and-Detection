# Radar-Target-Generation-and-Detection

The following picture shows the projects general setup.

<img src="img/project_layout.png" width="1359" height="772" />

System requirements
<img src="img/system_requirements.png" width="829" height="226" />

## FMCW Waveform Design

>Using the given system requirements, design
a FMCW waveform. Find its Bandwidth (B), chirp time (Tchirp) and slope of the chirp.

The following graphic shows the desired waveform which will be designed in the project. The red signal is the transmitted signal, and the blue one is the received signal, which is delayed in time by ![\tau](https://render.githubusercontent.com/render/math?math=%5Ctau). The chirp time is given by ![T_s](https://render.githubusercontent.com/render/math?math=T_s) and will be called ![T_{Chrip}](https://render.githubusercontent.com/render/math?math=T_%7BChrip%7D) in the following. The Bandwidth one chirp ranges over, is given ![B](https://render.githubusercontent.com/render/math?math=B) whereas ![\Delta f=f_b](https://render.githubusercontent.com/render/math?math=%5CDelta%20f%3Df_b) indicates the frequency shift of the received signal.

<img src="img/fmcw_waveform.png" width="865" height="352" />
[Image source: emagtech.com](http://www.emagtech.com/wiki/index.php/File:SysTUT7_5.png)

The bandwith, chrip time and slope getting calculated according to the following formula:

<!-- $$Bandwith (B) = \frac{speed\:of\:light}{2\:* \:range\:resolution}$$ -->
![Bandwith (B) = \frac{speed\:of\:light}{2\:*\:range\:resolution}](https://render.githubusercontent.com/render/math?math=Bandwith%20(B)%20%3D%20%5Cfrac%7Bspeed%5C%3Aof%5C%3Alight%7D%7B2%5C%3A*%5C%3Arange%5C%3Aresolution%7D)

<!-- $$T_{Chirp} = \frac{Chirp\:factor\:* \:2\:* \:max\:range}{speed\:of\:light}$$ -->
![T_{Chirp} = \frac{Chirp\:factor\:*\:2\:*\:max\:range}{speed\:of\:light}](https://render.githubusercontent.com/render/math?math=T_%7BChirp%7D%20%3D%20%5Cfrac%7BChirp%5C%3Afactor%5C%3A*%5C%3A2%5C%3A*%5C%3Amax%5C%3Arange%7D%7Bspeed%5C%3Aof%5C%3Alight%7D)

<!-- $$slope = \frac{Bandwith}{T_{Chirp}}$$ -->
![slope = \frac{Bandwith}{T_{Chirp}}](https://render.githubusercontent.com/render/math?math=slope%20%3D%20%5Cfrac%7BBandwith%7D%7BT_%7BChirp%7D%7D)


for the given system setup the values calculate as following:

<!-- $$B = \frac{c}{2\:* \:R_{res}} = \frac{3e^8\frac{m}{s}}{2\:* \:1m} = 150MHz$$ -->
![B = \frac{c}{2\:*\:R_{res}} = \frac{3e^8\frac{m}{s}}{2\:*\:1m} = 150MHz](https://render.githubusercontent.com/render/math?math=B%20%3D%20%5Cfrac%7Bc%7D%7B2%5C%3A*%5C%3AR_%7Bres%7D%7D%20%3D%20%5Cfrac%7B3e%5E8%5Cfrac%7Bm%7D%7Bs%7D%7D%7B2%5C%3A*%5C%3A1m%7D%20%3D%20150MHz)

<!-- $$T_{Chirp} = \frac{cf\:* \:2\:* \:R_{max}}{c} = \frac{5.5\:* \:2\:* \:200m}{3e^8\frac{m}{s}} = 7.3333e^{-6}s$$ -->
![T_{Chirp} = \frac{cf\:*\:2\:*\:R_{max}}{c} = \frac{5.5\:*\:2\:*\:200m}{3e^8\frac{m}{s}} = 7.3333e^{-6}s](https://render.githubusercontent.com/render/math?math=T_%7BChirp%7D%20%3D%20%5Cfrac%7Bcf%5C%3A*%5C%3A2%5C%3A*%5C%3AR_%7Bmax%7D%7D%7Bc%7D%20%3D%20%5Cfrac%7B5.5%5C%3A*%5C%3A2%5C%3A*%5C%3A200m%7D%7B3e%5E8%5Cfrac%7Bm%7D%7Bs%7D%7D%20%3D%207.3333e%5E%7B-6%7Ds)

<!-- $$slope = \frac{B}{T_{Chirp}} = \frac{150MHz}{7.3333e^{-6}s} = 2.0455e^{13}$$ -->
![slope = \frac{B}{T_{Chirp}} = \frac{150MHz}{7.3333e^{-6}s} = 2.0455e^{13}](https://render.githubusercontent.com/render/math?math=slope%20%3D%20%5Cfrac%7BB%7D%7BT_%7BChirp%7D%7D%20%3D%20%5Cfrac%7B150MHz%7D%7B7.3333e%5E%7B-6%7Ds%7D%20%3D%202.0455e%5E%7B13%7D)

>For given system requirements the calculated slope should be around 2e13



```
%chirp factor cf
cf = 5.5;

B = c / (2*Rres);

Tchirp = cf*2*Rmax/c;

slope = B/Tchirp;
```


## Simulation Loop / Signal Generation

>Simulate Target movement and calculate the beat or mixed signal for every timestamp.

The transmit signal ![T_x](https://render.githubusercontent.com/render/math?math=T_x) and the receive signal ![R_x](https://render.githubusercontent.com/render/math?math=R_x) are defined by the following formula where ![\tau](https://render.githubusercontent.com/render/math?math=%5Ctau) is the delay in time.

<!-- $$T_x = cos(2\pi(f_ct+\frac{\alpha t^2}{2}))$$ -->
![T_x = cos(2\pi(f_ct+\frac{\alpha t^2}{2}))](https://render.githubusercontent.com/render/math?math=T_x%20%3D%20cos(2%5Cpi(f_ct%2B%5Cfrac%7B%5Calpha%20t%5E2%7D%7B2%7D)))


<!--R_x = cos(2\pi(f_c(t-\tau)+\frac{\alpha (t-\tau)^2}{2}))-->
![R_x = cos(2\pi(f_c(t-\tau)+\frac{\alpha (t-\tau)^2}{2}))](https://render.githubusercontent.com/render/math?math=R_x%20%3D%20cos(2%5Cpi(f_c(t-%5Ctau)%2B%5Cfrac%7B%5Calpha%20(t-%5Ctau)%5E2%7D%7B2%7D)))

<!--T_x * R_x = cos(2\pi(\frac{2\alpha R}{c}t+\frac{2f_cvn}{c}t))$$)-->
![T_x * R_x = cos(2\pi(\frac{2\alpha R}{c}t+\frac{2f_cvn}{c}t))](https://render.githubusercontent.com/render/math?math=T_x%20*%20R_x%20%3D%20cos(2%5Cpi(%5Cfrac%7B2%5Calpha%20R%7D%7Bc%7Dt%2B%5Cfrac%7B2f_cvn%7D%7Bc%7Dt)))

>A beat signal should be generated such that once range FFT implemented, it gives the correct range i.e the initial position of target assigned with an error margin of +/- 10 meters.

```
%For each time stamp update the Range of the Target for constant velocity.
%range of target gets de/increased by const velocity * time passed
r_t(i) = target_dist + target_vel * t(i);
%the time delay is given by the distance from ego vehicle to target and
%back and the signals speed
td(i) = 2 * r_t(i) / c;
```
```
%For each time sample we need update the transmitted and
%received signal.
Tx(i) = cos(2*pi*(fc*t(i) + slope * t(i)^2 / 2.0 ));
Rx(i) = cos(2*pi*(fc*(t(i)-td(i)) + slope * (t(i)-td(i))^2 / 2.0 ));
```
## Range FFT (1st FFT)

>Implement the Range FFT on the Beat or Mixed Signal and plot the result.
```
%reshape the vector into Nr*Nd array. Nr and Nd here would also define the size of
%Range and Doppler FFT respectively.
beat = reshape(Mix,Nr,Nd);
```
```
%run the FFT on the beat signal along the range bins dimension (Nr) and
%normalize.
fft_beat = fft(beat,Nr);
fft_beat = fft_beat/Nr;
```
```
% Take the absolute value of FFT output
fft_beat = abs(fft_beat);
```
```
% Output of FFT is double sided signal, but we are interested in only one side of the spectrum.
% Hence we throw out half of the samples.
fft_beat = fft_beat(1:Nr/2);
```
```
%plotting the range
figure ('Name','Range from First FFT')
subplot(2,1,1)
```
```
% plot FFT output
plot(fft_beat);
axis ([0 200 0 1]);
title('Range from First FFT')
```

>A correct implementation should generate a peak at the correct range, i.e the
initial position of target assigned with an error margin of +/- 10 meters.

The following graph shows the result, stating an object at around 120m distance, which has been specified earlier.

<img src="img/range_first_fft.png" width="962" height="356" />

## Range Doppler Map

Code for the range doppler map has been rovided:

```
% The 2D FFT implementation is already provided here. This will run a 2DFFT
% on the mixed signal (beat signal) output and generate a range doppler
% map.You will implement CFAR on the generated RDM


% Range Doppler Map Generation.

% The output of the 2D FFT is an image that has reponse in the range and
% doppler FFT bins. So, it is important to convert the axis from bin sizes
% to range and doppler based on their Max values.

Mix=reshape(Mix,[Nr,Nd]);

% 2D FFT using the FFT size for both dimensions.
sig_fft2 = fft2(Mix,Nr,Nd);

% Taking just one side of signal from Range dimension.
sig_fft2 = sig_fft2(1:Nr/2,1:Nd);
sig_fft2 = fftshift (sig_fft2);
RDM = abs(sig_fft2);
RDM = 10*log10(RDM) ;

%use the surf function to plot the output of 2DFFT and to show axis in both
%dimensions
doppler_axis = linspace(-100,100,Nd);
range_axis = linspace(-200,200,Nr/2)*((Nr/2)/400);
figure,surf(doppler_axis,range_axis,RDM);
```

The image below shows the result for the defined target at 120m with a velocity of 35m/s.

<img src="img/range_doppler_map.png" width="1018" height="747" />

## 2D CFAR

>Implement the 2D CFAR process on the output of 2D FFT operation, i.e the Range Doppler Map.

> The 2D CFAR processing should be able to suppress the noise and separate
the target signal. The output should match the image shared in walkthrough.

>Determine the number of Training cells for each dimension. Similarly, pick the number of guard cells.

The following image shows the general principal of choosing the training and guard cell geometry around the **C**ell **U**nder **T**est - **CUT**.

<img src="img/2d_cfar.png" width="790" height="524" />
[Image source: electronicproducts.com](https://www.electronicproducts.com/Digital_ICs/Microprocessors_Microcontrollers_DSPs/Radar_signal_processing_fuels_automation_in_automotive_applications.aspx#)



Within this project the given setup has been chose:

```
%Select the number of Training Cells in both the dimensions.
T=[4,10];
%Select the number of Guard Cells in both dimensions around the Cell under
%test (CUT) for accurate estimation
G=[4,8];
```

>Slide the cell under test across the complete matrix. Make sure the CUT has margin for Training and Guard cells from the edges.

To slide across the matrix, two nested for loops have been use, where x and y are the coordinates in the matrix on the vertical and horizontal axis. They start and stop with an offset of the training and guard cells.

```
size = size(RDM);
rdm_size_x = size(1);
rdm_size_y = size(2);

for x = (T(1)+G(1)+1) : (rdm_size_x-(G(1)+T(1)))
   for y = (T(2)+G(2)+1) : (rdm_size_y-(G(2)+T(2)))
       % Use RDM[x,y] as the matrix from the output of 2D FFT for implementing CFAR
```

>For every iteration sum the signal level within all the training cells. To sum convert the value from logarithmic to linear using db2pow function.

First the overall training cell region gets been summed up, then the sum of the guard cell region gets been subtracted to stay with only the training cells.

```
noise_sum = sum(db2pow(RDM( i-T(1)-G(1) : i+T(1)+G(1), j-G(2)-T(2) : j+G(2)+T(2) )),'all') -...
            sum(db2pow(RDM(i-G(1) : i+G(1), j-G(2) : j+G(2) )),'all');
```

>Average the summed values for all of the training cells used. After averaging convert it back to logarithmic using pow2db.

To count the training cells a similar approach has been chosen. First overall cells for x and y direction get counted and multiplied to end up with the overall amount of cells. Then same has been done with only the guard cells, which finally will be subtracted from the overall amount.

```
num_Tcells = (2*( T(1)+G(1) ) +1) * (2*( T(2)+G(2) ) +1) ...
                    - (2* G(1) +1) * (2* G(2) +1);

       threshold = pow2db(noise_sum / num_Tcells);
```

>Further add the offset to it to determine the threshold.

```
threshold_cfar(i,j) = threshold;
```

 >Next, compare the signal under CUT against this threshold.
If the CUT level > threshold assign it a value of 1, else equate it to 0.

Finally the value for the CUT will be set to one if the threshold is been exceeded. All other cells will be left with the initial value zero.

```
if (CUT > threshold)
     signal_cfar(i,j) = 1;
end
%signal_cfar is initialized with zeros.
```

The result can be observed int the following image:

<img src="img/2d_cfar_result.png" width="978" height="773" />



>Create a CFAR README File

> Implementation steps for the 2D CFAR process.
Selection of Training, Guard cells and offset.
Steps taken to suppress the non-thresholded cells at the edges.
