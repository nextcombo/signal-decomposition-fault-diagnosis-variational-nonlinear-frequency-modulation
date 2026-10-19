# [Signal Decomposition, Fault Diagnosis]Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB Code]

Today, we introduce a novel signal decomposition method that is primarily applied in the field of mechanical signals and fault diagnosis. However, the document we screenshoted today pertains to communication-related signal processing. Interested students can also search for "VNCMD [Rapid Hoyer Spectral Diagram and VNC...D Transformation Frequency Rolling Bear Fault Diagnosis]" on CNKI to explore its application in the field of fault diagnosis.
At the same time, VNCMD can extract the time-frequency contour lines of signals with different frequency characteristics. Therefore, I consider it could also be applied to radar and communication signal sorting. Below, we will briefly introduce the corresponding principles. At the end, we will attach MATLAB simulation figures and main code (main user code will be uploaded at night).
MATLAB Simulation
MATLAB Code:
%%%%%%%%%%%%%% signal with intersecting IFs %%%%%%%%%%%%%%%%%\nclc\nclear\nclose all\nSampFreq = 2000;\nt = 0:1/SampFreq:1;\nSig1 = (1+0.5*cos(2*pi*t)).*cos(2*pi*(0.2 + 532*t -474*t.^2 + 369*t.^3));\nIF1 = 532 - 948*t + 1107*t.^2;\nSig2 = (1+0.5*cos(2*pi*t)).*cos(2*pi*(0.8+50*t + 525*t.^2 -300*t.^3));\nIF2 = 50 + 1050*t - 900*t.^2;\nSig = Sig1+Sig2;\nfigure\nset(gcf,'Position',[20 100 320 250]);      \nset(gcf,'Color','w'); \nplot(t,Sig,'linewidth',1);\nxlabel('Time / Sec','FontSize',12,'FontName','Times New Roman');\nylabel('Amplitude','FontSize',12,'FontName','Times New Roman');\nset(gca,'xtick',[0 0.2 0.4 0.6 0.8 1]);\nset(gca,'FontSize',12)\nset(gca,'linewidth',1);\naxis([0 1 -4 4])\n%% STFT\nRatio = 0;\nfigure\n[Spec,f] = STFT(Sig',SampFreq,512,256);\nimagesc(t,f,abs(Spec)); \naxis([0 1 0 700]);\nset(gcf,'Position',[20 100 320 250]);   \nxlabel('Time / Sec','FontSize',12,'FontName','Times New Roman');\nylabel('Frequency / Hz','FontSize',12,'FontName','Times New Roman');\nset(gca,'xtick',[0 0.2 0.4 0.6 0.8 1]);\nset(gca,'YDir','normal')\nset(gca,'FontSize',12);\nset(gcf,'Color','w');  \n%% parameter setting\niniIF = [700*ones(1,length(t));20*ones(1,length(t))];% initial guess for the IFs for the three signal modes\nalpha = 5e-6; \nbeta = 1e-6; % this parameter can be smaller which will be helpful for the convergence, but it may cannot properly track fast varying IFs\nvar = 0;% noise variance\ntol = 1e-8;%\ntic\n[IFmset IA smset] = VNCMD(Sig,SampFreq,iniIF,alpha,beta,var,tol);\ntoc\n​\n%% Relative errors of the finally estimated IFs\n   RE1 =  norm(IFmset(1,:,end)-IF1)/norm(IF1)\n   RE2 =  norm(IFmset(2,:,end)-IF2)/norm(IF2)\n%% estimated IF\nfigure\nplot(t,[IF1;IF2],'b','linewidth',3) % true IFs\nhold on\nplot(t,IFmset(:,:,end),'r','linewidth',3) % finally estimated IFs\nset(gcf,'Position',[20 100 640 500]);   \nxlabel('Time / Sec','FontSize',24,'FontName','Times New Roman');\nylabel('Frequency / Hz','FontSize',24,'FontName','Times New Roman');\nset(gca,'xtick',[0 0.2 0.4 0.6 0.8 1]);\nset(gca,'YDir','normal')\nset(gca,'FontSize',24);\nset(gca,'linewidth',2);\nset(gcf,'Color','w');  \naxis([0 1 0 800])\n%% Reconstructed modes\nfigure\nset(gcf,'Position',[20 100 640 200]);      \nset(gcf,'Color','w'); \nplot(t,smset(1,:,end),'linewidth',2)  % estimated mode\nhold on\nplot(t,Sig1 - smset(1,:,end),'k','linewidth',2) % estimation errors\nhold on\nplot(t,IA(1,:),'r','linewidth',3) % estimated IAs\nxlabel('Time / Sec','FontSize',24,'FontName','Times New Roman');\nylabel('m1','FontSize',24,'FontName','Times New Roman');set(gca,'YDir','normal')\nset(gca,'FontSize',24);\nset(gca,'linewidth',2);  \naxis([0 1 -1.9 1.9])\n​\nfigure\nset(gcf,'Position',[20 100 640 200]);      \nset(gcf,'Color','w'); \nplot(t,smset(2,:,end),'linewidth',2)  % estimated mode\nhold on\nplot(t,Sig2 - smset(2,:,end),'k','linewidth',2)  % estimation errors\nhold on\nplot(t,IA(2,:),'r','linewidth',3) % estimated IAs\nxlabel('Time / Sec','FontSize',24,'FontName','Times New Roman');\nylabel('m2','FontSize',24,'FontName','Times New Roman');set(gca,'YDir','normal')\nset(gca,'FontSize',24);\nset(gca,'linewidth',2);  \naxis([0 1 -1.9 1.9])\n​\n  

## Images

![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](01.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](02.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](03.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](04.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](05.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](06.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](07.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](08.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](09.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](10.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](11.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](12.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](13.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](14.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](15.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](16.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](17.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](18.jpg)
![(Signal Decomposition, Fault Diagnosis)Variational Nonlinear Frequency Modulation Mode Decomposition (VNCMD) [with MATLAB code]](19.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
