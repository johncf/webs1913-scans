# Webster's International Dictionary of the English Language, 1898

A.k.a **Webster's Unabridged Dictionary** by Noah Webster

Comprising the issues of 1864, 1879, and 1884

Revised and enlarged under the supervision of Noah Porter

## Construction

0. Download the ebook from archive.org (as pdf):

   <https://archive.org/details/webstersinternat00port>

1. Generate images from pdf (req: _poppler_):

   ```sh
   pdfimages -p -jp2 internat00port.pdf w00-raw/i
   ```

2. Rename for consistency:

   ```sh
   ls i-???-???.??? | awk '{printf "mv -bS.bak %s p0%s-%s.%s\n", $0, substr($0, 3, 3), substr($0, 7, 3) + 3 - 3*substr($0, 3, 3), substr($0, 11, 3)}' | bash
   ls i-???-????.??? | awk '{printf "mv -bS.bak %s p0%s-%s.%s\n", $0, substr($0, 3, 3), substr($0, 7, 4) + 3 - 3*substr($0, 3, 3), substr($0, 12, 3)}' | bash
   ls i-????-????.??? | awk '{printf "mv -bS.bak %s p%s-%s.%s\n", $0, substr($0, 3, 4), substr($0, 8, 4) + 3 - 3*substr($0, 3, 4), substr($0, 13, 3)}' | bash
   ```

3. Combine mask images, and copy bg (req: _ImageMagick_):

   ```sh
   mkdir final
   for i in $(seq -f'%04g' 23 32)
   do echo $i; convert p$i-1.jp2 p$i-2.pbm -channel-fx '| gray=>alpha' -quality 95 -colors 255 -alpha Background final/p$i-ol.png
   done
   for i in $(cat <(seq -f'%04g' 1 22) <(seq -f'%04g' 33 2208))
   do echo $i; convert p$i-1.jp2 p$i-2.pbm -colorspace Gray -channel-fx '| gray=>alpha' -quality 92 -alpha Background -colors 16 final/p$i-ol.png
   done
   seq -f 'i=%04g; cp p$i-0.jp2 final/p$i-bg.jp2' 2208 | bash
   ```

4. Separate into `0.plates` `1.intro` `2.main` `3.appx` `9.extras`

5. Re-number files based on page numbers. As an example (from `2.main`):

   ```sh
   paste -d ' ' <(seq -f 'mv p%04g-ol.png' 119 1799) <(seq -f 'p%04g-ol.png' 1 1681) | bash
   paste -d ' ' <(seq -f 'mv p%04g-bg.jp2' 119 1799) <(seq -f 'p%04g-bg.jp2' 1 1681) | bash
   ```

5. Separate `2.main` into `2.xx`

   ```sh
   for i in {a,b,c,d,e,f,gh,i,jkl,m,no,p,qr,s,t,uv,wxyz}
   do mkdir 2.$i; done && cd 2.main
   seq -f 'mv p%04g-??.??? ../2.a/'       1  108 | bash
   seq -f 'mv p%04g-??.??? ../2.b/'     109  198 | bash
   seq -f 'mv p%04g-??.??? ../2.c/'     199  363 | bash
   seq -f 'mv p%04g-??.??? ../2.d/'     364  463 | bash
   seq -f 'mv p%04g-??.??? ../2.e/'     464  534 | bash
   seq -f 'mv p%04g-??.??? ../2.f/'     535  605 | bash
   seq -f 'mv p%04g-??.??? ../2.gh/'    606  722 | bash
   seq -f 'mv p%04g-??.??? ../2.i/'     723  793 | bash
   seq -f 'mv p%04g-??.??? ../2.jkl/'   794  877 | bash
   seq -f 'mv p%04g-??.??? ../2.m/'     878  960 | bash
   seq -f 'mv p%04g-??.??? ../2.no/'    961 1027 | bash
   seq -f 'mv p%04g-??.??? ../2.p/'    1028 1170 | bash
   seq -f 'mv p%04g-??.??? ../2.qr/'   1171 1263 | bash
   seq -f 'mv p%04g-??.??? ../2.s/'    1264 1465 | bash
   seq -f 'mv p%04g-??.??? ../2.t/'    1466 1559 | bash
   seq -f 'mv p%04g-??.??? ../2.uv/'   1560 1620 | bash
   seq -f 'mv p%04g-??.??? ../2.wxyz/' 1621 1681 | bash
   ```

## License

This work is believed to be in the public domain. For more details, visit the archive.org link above.
