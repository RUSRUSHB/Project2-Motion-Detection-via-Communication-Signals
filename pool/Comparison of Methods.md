# Comparison of Methods

Since the data are too large, the optimization is crucial. At first, our team made use of the matrix operations recommended in MATLAB, which contains:

```matlab
for j=1:41
    corMat(i,j)=abs(sum(ys.*conj(yr).*exp_component(length(ys),Fd(j),fs)));
end
```

However, the time cost is too large. So, we try accelerate the calculation.
Define:
$$
x[n]:=y_{surf}[nT_s]y^*_{ref}[nT_s-\tau]
$$
Note that now the ambiguity function looks like Fourier transform of $x[n]$:
$$
Cor(\tau, f_D)=\sum^{N-1}_{n=0}x[n]e^{-j2\pi f_DnT_s}
$$

Recall that the k-th term of Fourier Transform is defined as:
$$
\sum^{N-1}_{n=0}x[n]e^{-j2\pi kn/N}
$$
When:
$$
-j2\pi f_DnT_s=-j2\pi kn/N
\iff k=f_DT_sN
$$
they are the same.

Therefore, we can use FFT to accelerate the calculation.

```matlab
Y=fft(ys.*conj(yr));
corMat(i,:)=fftshift(abs(Y(fix(Fd.*length(ys)/fs)+21)));
```

Note that `fftshift` and `+20` is used to make the index of negative frequencies legal, and `+1` is used because the index of MATLAB starts from 1.

***

It's also a good idea to examine how efficient does MATLAB optimize the matrix operations. We try to use the `for` loop to calculate the ambiguity function:

```matlab
for j=1:41
    for k=1:length(ys)
            corMat(i,j)=corMat(i,j)+ys(k).*conj(yr(k)).*exp(-2j*pi*Fd(j)*k/fs);
    end
    corMat(i,j)=abs(corMat(i,j));
end
```

//TODO: figures

From the result we can see that MATLAB is really efficient in matrix operations, and FFT is a good way to accelerate the calculation. We can also notice minor differences among the figures, which might be caused by errors in float number calculations.