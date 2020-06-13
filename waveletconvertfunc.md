# Discrete-two-dimensional-fast-wavelet-transform
function [imgs] = waveletconvertfunc1(filepath,rows,cols,bands,outputfile)
%WAVELETCONVERTFUNC1
%   Contents
%Is image file exist?
if(exist(filepath,'file') == 0)
 error('image file unexist');
end
%Is hdr file exist?
hdrfile = strrep(filepath,'.dat','.hdr');
if(exist(hdrfile,'file') == 0)
 error('header file unexist');
end
%read image 
imgs = multibandread(filepath,[rows,cols,bands],'single',0,'bip','ieee-le');
%wavelet transform per band
for i=1:bands
[c,s] = wavedec2(imgs(:,:,i),2,'sym5');
[thr,sorh,keepapp] = ddencmp('den','wv',imgs(:,:,i));
[img,~,~,~,~] = wdencmp('gbl',c,s,'sym5',2,thr,sorh,keepapp);
imgs(:,:,i)= img;
end
%output image file
multibandwrite(imgs,outputfile,'bsq','precision','single','offset',0,'machfmt','ieee-le');
outputhdrfile = strrep(outputfile,'.dat','.hdr');
%generate hdr file in response to output file
copyfile(hdrfile, outputhdrfile);
end
