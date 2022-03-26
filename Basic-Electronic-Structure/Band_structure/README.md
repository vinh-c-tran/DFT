# Band Structure 

For a certain structure we can ask about the energy momentum dispersion relation. 

## Quantum Espresso Band Structure 
We need to run two calculations to end up with the band structure data
1. Run a standard `pw.x` scf calculation. 
2. Run a `pw.x` non-scf calculation with the `&CONTROL` card `calculation = 'bands'`. For this, we need to also specify a path for k-points. 
3. Run a `bands.x` calculation 


### First Step: `pw.x` non-scf band calculation
We can start with the standard input file for an scf calculation. 
```fortran
 &CONTROL
    calculation = 'scf',
    prefix      = 'graphene',
    outdir      = './temp',
    pseudo_dir  = '/Users/vinhtran/Documents/GitHub/DFT/pseudos' 
 /

 &SYSTEM
    ibrav     = 4,
    celldm(1) = 4.654,
    celldm(3) = 3.0,
    nat  = 2,
    ntyp = 1,
    ecutwfc = 20.0,
    ecutrho = 200.0, 

!    occupations = 'smearing',
!    smearing = 'gaussian',
!    degauss = 0.01,
 /
 
 &ELECTRONS
    conv_thr = 1.0d-8
 /
 
ATOMIC_SPECIES
   C  12.0107 C.pbe-n-kjpaw_psl.1.0.0.UPF
   
ATOMIC_POSITIONS alat
   C    0.000000    0.0000000   0.000000
   C    0.000000    0.5773503   0.000000
   
K_POINTS automatic
   9 9 1   0 0 0
```

### Second Step: Non-Scf Band Calculation 
For this step, we need to use an external tool to specify the `k-path` that we use for this calculation. We can use a super convenient tool called SeeK-path at ht

We pass to it our pw.x scf input file and it will generate a pw.x nscf bands calculation input file for us. In particular, we get the following direct output from the webtool. To this we then need to add our other variables such as directory and pseudopotential location. 

```fortran
&CONTROL
    calculation = 'bands'
    <...>
/
&SYSTEM
    ibrav = 0
    nat = 2
    ntyp = 1
    <...>
/
&ELECTRONS
    <...>
/
ATOMIC_SPECIES
C    <MASS_HERE> <PSEUDO_HERE>.UPF
ATOMIC_POSITIONS angstrom
C        0.0000000000     1.4218928902     0.0000000000
C        1.2313953644     0.7109464451     0.0000000000
K_POINTS crystal
365
    0.0000000000     0.0000000000     0.0000000000 1
    0.0087719298     0.0000000000     0.0000000000 1
    0.0175438596     0.0000000000     0.0000000000 1
    0.0263157895     0.0000000000     0.0000000000 1
    0.0350877193     0.0000000000     0.0000000000 1
    0.0438596491     0.0000000000     0.0000000000 1
    0.0526315789     0.0000000000     0.0000000000 1
    0.0614035088     0.0000000000     0.0000000000 1
    0.0701754386     0.0000000000     0.0000000000 1
    0.0789473684     0.0000000000     0.0000000000 1
    0.0877192982     0.0000000000     0.0000000000 1
    0.0964912281     0.0000000000     0.0000000000 1
    0.1052631579     0.0000000000     0.0000000000 1
    0.1140350877     0.0000000000     0.0000000000 1
    0.1228070175     0.0000000000     0.0000000000 1
    0.1315789474     0.0000000000     0.0000000000 1
    0.1403508772     0.0000000000     0.0000000000 1
    0.1491228070     0.0000000000     0.0000000000 1
    0.1578947368     0.0000000000     0.0000000000 1
    0.1666666667     0.0000000000     0.0000000000 1
    0.1754385965     0.0000000000     0.0000000000 1
    0.1842105263     0.0000000000     0.0000000000 1
    0.1929824561     0.0000000000     0.0000000000 1
    0.2017543860     0.0000000000     0.0000000000 1
    0.2105263158     0.0000000000     0.0000000000 1
    0.2192982456     0.0000000000     0.0000000000 1
    0.2280701754     0.0000000000     0.0000000000 1
    0.2368421053     0.0000000000     0.0000000000 1
    0.2456140351     0.0000000000     0.0000000000 1
    0.2543859649     0.0000000000     0.0000000000 1
    0.2631578947     0.0000000000     0.0000000000 1
    0.2719298246     0.0000000000     0.0000000000 1
    0.2807017544     0.0000000000     0.0000000000 1
    0.2894736842     0.0000000000     0.0000000000 1
    0.2982456140     0.0000000000     0.0000000000 1
    0.3070175439     0.0000000000     0.0000000000 1
    0.3157894737     0.0000000000     0.0000000000 1
    0.3245614035     0.0000000000     0.0000000000 1
    0.3333333333     0.0000000000     0.0000000000 1
    0.3421052632     0.0000000000     0.0000000000 1
    0.3508771930     0.0000000000     0.0000000000 1
    0.3596491228     0.0000000000     0.0000000000 1
    0.3684210526     0.0000000000     0.0000000000 1
    0.3771929825     0.0000000000     0.0000000000 1
    0.3859649123     0.0000000000     0.0000000000 1
    0.3947368421     0.0000000000     0.0000000000 1
    0.4035087719     0.0000000000     0.0000000000 1
    0.4122807018     0.0000000000     0.0000000000 1
    0.4210526316     0.0000000000     0.0000000000 1
    0.4298245614     0.0000000000     0.0000000000 1
    0.4385964912     0.0000000000     0.0000000000 1
    0.4473684211     0.0000000000     0.0000000000 1
    0.4561403509     0.0000000000     0.0000000000 1
    0.4649122807     0.0000000000     0.0000000000 1
    0.4736842105     0.0000000000     0.0000000000 1
    0.4824561404     0.0000000000     0.0000000000 1
    0.4912280702     0.0000000000     0.0000000000 1
    0.5000000000     0.0000000000     0.0000000000 1
    0.4949494949     0.0101010101     0.0000000000 1
    0.4898989899     0.0202020202     0.0000000000 1
    0.4848484848     0.0303030303     0.0000000000 1
    0.4797979798     0.0404040404     0.0000000000 1
    0.4747474747     0.0505050505     0.0000000000 1
    0.4696969697     0.0606060606     0.0000000000 1
    0.4646464646     0.0707070707     0.0000000000 1
    0.4595959596     0.0808080808     0.0000000000 1
    0.4545454545     0.0909090909     0.0000000000 1
    0.4494949495     0.1010101010     0.0000000000 1
    0.4444444444     0.1111111111     0.0000000000 1
    0.4393939394     0.1212121212     0.0000000000 1
    0.4343434343     0.1313131313     0.0000000000 1
    0.4292929293     0.1414141414     0.0000000000 1
    0.4242424242     0.1515151515     0.0000000000 1
    0.4191919192     0.1616161616     0.0000000000 1
    0.4141414141     0.1717171717     0.0000000000 1
    0.4090909091     0.1818181818     0.0000000000 1
    0.4040404040     0.1919191919     0.0000000000 1
    0.3989898990     0.2020202020     0.0000000000 1
    0.3939393939     0.2121212121     0.0000000000 1
    0.3888888889     0.2222222222     0.0000000000 1
    0.3838383838     0.2323232323     0.0000000000 1
    0.3787878788     0.2424242424     0.0000000000 1
    0.3737373737     0.2525252525     0.0000000000 1
    0.3686868687     0.2626262626     0.0000000000 1
    0.3636363636     0.2727272727     0.0000000000 1
    0.3585858586     0.2828282828     0.0000000000 1
    0.3535353535     0.2929292929     0.0000000000 1
    0.3484848485     0.3030303030     0.0000000000 1
    0.3434343434     0.3131313131     0.0000000000 1
    0.3383838384     0.3232323232     0.0000000000 1
    0.3333333333     0.3333333333     0.0000000000 1
    0.3283582090     0.3283582090     0.0000000000 1
    0.3233830846     0.3233830846     0.0000000000 1
    0.3184079602     0.3184079602     0.0000000000 1
    0.3134328358     0.3134328358     0.0000000000 1
    0.3084577114     0.3084577114     0.0000000000 1
    0.3034825871     0.3034825871     0.0000000000 1
    0.2985074627     0.2985074627     0.0000000000 1
    0.2935323383     0.2935323383     0.0000000000 1
    0.2885572139     0.2885572139     0.0000000000 1
    0.2835820896     0.2835820896     0.0000000000 1
    0.2786069652     0.2786069652     0.0000000000 1
    0.2736318408     0.2736318408     0.0000000000 1
    0.2686567164     0.2686567164     0.0000000000 1
    0.2636815920     0.2636815920     0.0000000000 1
    0.2587064677     0.2587064677     0.0000000000 1
    0.2537313433     0.2537313433     0.0000000000 1
    0.2487562189     0.2487562189     0.0000000000 1
    0.2437810945     0.2437810945     0.0000000000 1
    0.2388059701     0.2388059701     0.0000000000 1
    0.2338308458     0.2338308458     0.0000000000 1
    0.2288557214     0.2288557214     0.0000000000 1
    0.2238805970     0.2238805970     0.0000000000 1
    0.2189054726     0.2189054726     0.0000000000 1
    0.2139303483     0.2139303483     0.0000000000 1
    0.2089552239     0.2089552239     0.0000000000 1
    0.2039800995     0.2039800995     0.0000000000 1
    0.1990049751     0.1990049751     0.0000000000 1
    0.1940298507     0.1940298507     0.0000000000 1
    0.1890547264     0.1890547264     0.0000000000 1
    0.1840796020     0.1840796020     0.0000000000 1
    0.1791044776     0.1791044776     0.0000000000 1
    0.1741293532     0.1741293532     0.0000000000 1
    0.1691542289     0.1691542289     0.0000000000 1
    0.1641791045     0.1641791045     0.0000000000 1
    0.1592039801     0.1592039801     0.0000000000 1
    0.1542288557     0.1542288557     0.0000000000 1
    0.1492537313     0.1492537313     0.0000000000 1
    0.1442786070     0.1442786070     0.0000000000 1
    0.1393034826     0.1393034826     0.0000000000 1
    0.1343283582     0.1343283582     0.0000000000 1
    0.1293532338     0.1293532338     0.0000000000 1
    0.1243781095     0.1243781095     0.0000000000 1
    0.1194029851     0.1194029851     0.0000000000 1
    0.1144278607     0.1144278607     0.0000000000 1
    0.1094527363     0.1094527363     0.0000000000 1
    0.1044776119     0.1044776119     0.0000000000 1
    0.0995024876     0.0995024876     0.0000000000 1
    0.0945273632     0.0945273632     0.0000000000 1
    0.0895522388     0.0895522388     0.0000000000 1
    0.0845771144     0.0845771144     0.0000000000 1
    0.0796019900     0.0796019900     0.0000000000 1
    0.0746268657     0.0746268657     0.0000000000 1
    0.0696517413     0.0696517413     0.0000000000 1
    0.0646766169     0.0646766169     0.0000000000 1
    0.0597014925     0.0597014925     0.0000000000 1
    0.0547263682     0.0547263682     0.0000000000 1
    0.0497512438     0.0497512438     0.0000000000 1
    0.0447761194     0.0447761194     0.0000000000 1
    0.0398009950     0.0398009950     0.0000000000 1
    0.0348258706     0.0348258706     0.0000000000 1
    0.0298507463     0.0298507463     0.0000000000 1
    0.0248756219     0.0248756219     0.0000000000 1
    0.0199004975     0.0199004975     0.0000000000 1
    0.0149253731     0.0149253731     0.0000000000 1
    0.0099502488     0.0099502488     0.0000000000 1
    0.0049751244     0.0049751244     0.0000000000 1
    0.0000000000     0.0000000000     0.0000000000 1
    0.0000000000     0.0000000000     0.0312500000 1
    0.0000000000     0.0000000000     0.0625000000 1
    0.0000000000     0.0000000000     0.0937500000 1
    0.0000000000     0.0000000000     0.1250000000 1
    0.0000000000     0.0000000000     0.1562500000 1
    0.0000000000     0.0000000000     0.1875000000 1
    0.0000000000     0.0000000000     0.2187500000 1
    0.0000000000     0.0000000000     0.2500000000 1
    0.0000000000     0.0000000000     0.2812500000 1
    0.0000000000     0.0000000000     0.3125000000 1
    0.0000000000     0.0000000000     0.3437500000 1
    0.0000000000     0.0000000000     0.3750000000 1
    0.0000000000     0.0000000000     0.4062500000 1
    0.0000000000     0.0000000000     0.4375000000 1
    0.0000000000     0.0000000000     0.4687500000 1
    0.0000000000     0.0000000000     0.5000000000 1
    0.0087719298     0.0000000000     0.5000000000 1
    0.0175438596     0.0000000000     0.5000000000 1
    0.0263157895     0.0000000000     0.5000000000 1
    0.0350877193     0.0000000000     0.5000000000 1
    0.0438596491     0.0000000000     0.5000000000 1
    0.0526315789     0.0000000000     0.5000000000 1
    0.0614035088     0.0000000000     0.5000000000 1
    0.0701754386     0.0000000000     0.5000000000 1
    0.0789473684     0.0000000000     0.5000000000 1
    0.0877192982     0.0000000000     0.5000000000 1
    0.0964912281     0.0000000000     0.5000000000 1
    0.1052631579     0.0000000000     0.5000000000 1
    0.1140350877     0.0000000000     0.5000000000 1
    0.1228070175     0.0000000000     0.5000000000 1
    0.1315789474     0.0000000000     0.5000000000 1
    0.1403508772     0.0000000000     0.5000000000 1
    0.1491228070     0.0000000000     0.5000000000 1
    0.1578947368     0.0000000000     0.5000000000 1
    0.1666666667     0.0000000000     0.5000000000 1
    0.1754385965     0.0000000000     0.5000000000 1
    0.1842105263     0.0000000000     0.5000000000 1
    0.1929824561     0.0000000000     0.5000000000 1
    0.2017543860     0.0000000000     0.5000000000 1
    0.2105263158     0.0000000000     0.5000000000 1
    0.2192982456     0.0000000000     0.5000000000 1
    0.2280701754     0.0000000000     0.5000000000 1
    0.2368421053     0.0000000000     0.5000000000 1
    0.2456140351     0.0000000000     0.5000000000 1
    0.2543859649     0.0000000000     0.5000000000 1
    0.2631578947     0.0000000000     0.5000000000 1
    0.2719298246     0.0000000000     0.5000000000 1
    0.2807017544     0.0000000000     0.5000000000 1
    0.2894736842     0.0000000000     0.5000000000 1
    0.2982456140     0.0000000000     0.5000000000 1
    0.3070175439     0.0000000000     0.5000000000 1
    0.3157894737     0.0000000000     0.5000000000 1
    0.3245614035     0.0000000000     0.5000000000 1
    0.3333333333     0.0000000000     0.5000000000 1
    0.3421052632     0.0000000000     0.5000000000 1
    0.3508771930     0.0000000000     0.5000000000 1
    0.3596491228     0.0000000000     0.5000000000 1
    0.3684210526     0.0000000000     0.5000000000 1
    0.3771929825     0.0000000000     0.5000000000 1
    0.3859649123     0.0000000000     0.5000000000 1
    0.3947368421     0.0000000000     0.5000000000 1
    0.4035087719     0.0000000000     0.5000000000 1
    0.4122807018     0.0000000000     0.5000000000 1
    0.4210526316     0.0000000000     0.5000000000 1
    0.4298245614     0.0000000000     0.5000000000 1
    0.4385964912     0.0000000000     0.5000000000 1
    0.4473684211     0.0000000000     0.5000000000 1
    0.4561403509     0.0000000000     0.5000000000 1
    0.4649122807     0.0000000000     0.5000000000 1
    0.4736842105     0.0000000000     0.5000000000 1
    0.4824561404     0.0000000000     0.5000000000 1
    0.4912280702     0.0000000000     0.5000000000 1
    0.5000000000     0.0000000000     0.5000000000 1
    0.4949494949     0.0101010101     0.5000000000 1
    0.4898989899     0.0202020202     0.5000000000 1
    0.4848484848     0.0303030303     0.5000000000 1
    0.4797979798     0.0404040404     0.5000000000 1
    0.4747474747     0.0505050505     0.5000000000 1
    0.4696969697     0.0606060606     0.5000000000 1
    0.4646464646     0.0707070707     0.5000000000 1
    0.4595959596     0.0808080808     0.5000000000 1
    0.4545454545     0.0909090909     0.5000000000 1
    0.4494949495     0.1010101010     0.5000000000 1
    0.4444444444     0.1111111111     0.5000000000 1
    0.4393939394     0.1212121212     0.5000000000 1
    0.4343434343     0.1313131313     0.5000000000 1
    0.4292929293     0.1414141414     0.5000000000 1
    0.4242424242     0.1515151515     0.5000000000 1
    0.4191919192     0.1616161616     0.5000000000 1
    0.4141414141     0.1717171717     0.5000000000 1
    0.4090909091     0.1818181818     0.5000000000 1
    0.4040404040     0.1919191919     0.5000000000 1
    0.3989898990     0.2020202020     0.5000000000 1
    0.3939393939     0.2121212121     0.5000000000 1
    0.3888888889     0.2222222222     0.5000000000 1
    0.3838383838     0.2323232323     0.5000000000 1
    0.3787878788     0.2424242424     0.5000000000 1
    0.3737373737     0.2525252525     0.5000000000 1
    0.3686868687     0.2626262626     0.5000000000 1
    0.3636363636     0.2727272727     0.5000000000 1
    0.3585858586     0.2828282828     0.5000000000 1
    0.3535353535     0.2929292929     0.5000000000 1
    0.3484848485     0.3030303030     0.5000000000 1
    0.3434343434     0.3131313131     0.5000000000 1
    0.3383838384     0.3232323232     0.5000000000 1
    0.3333333333     0.3333333333     0.5000000000 1
    0.3283582090     0.3283582090     0.5000000000 1
    0.3233830846     0.3233830846     0.5000000000 1
    0.3184079602     0.3184079602     0.5000000000 1
    0.3134328358     0.3134328358     0.5000000000 1
    0.3084577114     0.3084577114     0.5000000000 1
    0.3034825871     0.3034825871     0.5000000000 1
    0.2985074627     0.2985074627     0.5000000000 1
    0.2935323383     0.2935323383     0.5000000000 1
    0.2885572139     0.2885572139     0.5000000000 1
    0.2835820896     0.2835820896     0.5000000000 1
    0.2786069652     0.2786069652     0.5000000000 1
    0.2736318408     0.2736318408     0.5000000000 1
    0.2686567164     0.2686567164     0.5000000000 1
    0.2636815920     0.2636815920     0.5000000000 1
    0.2587064677     0.2587064677     0.5000000000 1
    0.2537313433     0.2537313433     0.5000000000 1
    0.2487562189     0.2487562189     0.5000000000 1
    0.2437810945     0.2437810945     0.5000000000 1
    0.2388059701     0.2388059701     0.5000000000 1
    0.2338308458     0.2338308458     0.5000000000 1
    0.2288557214     0.2288557214     0.5000000000 1
    0.2238805970     0.2238805970     0.5000000000 1
    0.2189054726     0.2189054726     0.5000000000 1
    0.2139303483     0.2139303483     0.5000000000 1
    0.2089552239     0.2089552239     0.5000000000 1
    0.2039800995     0.2039800995     0.5000000000 1
    0.1990049751     0.1990049751     0.5000000000 1
    0.1940298507     0.1940298507     0.5000000000 1
    0.1890547264     0.1890547264     0.5000000000 1
    0.1840796020     0.1840796020     0.5000000000 1
    0.1791044776     0.1791044776     0.5000000000 1
    0.1741293532     0.1741293532     0.5000000000 1
    0.1691542289     0.1691542289     0.5000000000 1
    0.1641791045     0.1641791045     0.5000000000 1
    0.1592039801     0.1592039801     0.5000000000 1
    0.1542288557     0.1542288557     0.5000000000 1
    0.1492537313     0.1492537313     0.5000000000 1
    0.1442786070     0.1442786070     0.5000000000 1
    0.1393034826     0.1393034826     0.5000000000 1
    0.1343283582     0.1343283582     0.5000000000 1
    0.1293532338     0.1293532338     0.5000000000 1
    0.1243781095     0.1243781095     0.5000000000 1
    0.1194029851     0.1194029851     0.5000000000 1
    0.1144278607     0.1144278607     0.5000000000 1
    0.1094527363     0.1094527363     0.5000000000 1
    0.1044776119     0.1044776119     0.5000000000 1
    0.0995024876     0.0995024876     0.5000000000 1
    0.0945273632     0.0945273632     0.5000000000 1
    0.0895522388     0.0895522388     0.5000000000 1
    0.0845771144     0.0845771144     0.5000000000 1
    0.0796019900     0.0796019900     0.5000000000 1
    0.0746268657     0.0746268657     0.5000000000 1
    0.0696517413     0.0696517413     0.5000000000 1
    0.0646766169     0.0646766169     0.5000000000 1
    0.0597014925     0.0597014925     0.5000000000 1
    0.0547263682     0.0547263682     0.5000000000 1
    0.0497512438     0.0497512438     0.5000000000 1
    0.0447761194     0.0447761194     0.5000000000 1
    0.0398009950     0.0398009950     0.5000000000 1
    0.0348258706     0.0348258706     0.5000000000 1
    0.0298507463     0.0298507463     0.5000000000 1
    0.0248756219     0.0248756219     0.5000000000 1
    0.0199004975     0.0199004975     0.5000000000 1
    0.0149253731     0.0149253731     0.5000000000 1
    0.0099502488     0.0099502488     0.5000000000 1
    0.0049751244     0.0049751244     0.5000000000 1
    0.0000000000     0.0000000000     0.5000000000 1
    0.5000000000     0.0000000000     0.5000000000 1
    0.5000000000     0.0000000000     0.4687500000 1
    0.5000000000     0.0000000000     0.4375000000 1
    0.5000000000     0.0000000000     0.4062500000 1
    0.5000000000     0.0000000000     0.3750000000 1
    0.5000000000     0.0000000000     0.3437500000 1
    0.5000000000     0.0000000000     0.3125000000 1
    0.5000000000     0.0000000000     0.2812500000 1
    0.5000000000     0.0000000000     0.2500000000 1
    0.5000000000     0.0000000000     0.2187500000 1
    0.5000000000     0.0000000000     0.1875000000 1
    0.5000000000     0.0000000000     0.1562500000 1
    0.5000000000     0.0000000000     0.1250000000 1
    0.5000000000     0.0000000000     0.0937500000 1
    0.5000000000     0.0000000000     0.0625000000 1
    0.5000000000     0.0000000000     0.0312500000 1
    0.5000000000     0.0000000000     0.0000000000 1
    0.3333333333     0.3333333333     0.5000000000 1
    0.3333333333     0.3333333333     0.4687500000 1
    0.3333333333     0.3333333333     0.4375000000 1
    0.3333333333     0.3333333333     0.4062500000 1
    0.3333333333     0.3333333333     0.3750000000 1
    0.3333333333     0.3333333333     0.3437500000 1
    0.3333333333     0.3333333333     0.3125000000 1
    0.3333333333     0.3333333333     0.2812500000 1
    0.3333333333     0.3333333333     0.2500000000 1
    0.3333333333     0.3333333333     0.2187500000 1
    0.3333333333     0.3333333333     0.1875000000 1
    0.3333333333     0.3333333333     0.1562500000 1
    0.3333333333     0.3333333333     0.1250000000 1
    0.3333333333     0.3333333333     0.0937500000 1
    0.3333333333     0.3333333333     0.0625000000 1
    0.3333333333     0.3333333333     0.0312500000 1
    0.3333333333     0.3333333333     0.0000000000 1
CELL_PARAMETERS angstrom
    2.4627907288     0.0000000000     0.0000000000
   -1.2313953644     2.1328393353     0.0000000000
    0.0000000000     0.0000000000     7.3883721863
   ```

## Part 3: `bands.x` Calculation 
Finally, we need to call a `bands.x` calculation where we pass to `bands.x` and input file of the form (where again we need that empty line at the bottom) 
```fortran 
&bands 
    prefix = 'gp' 
    outdir = './temp' 
    filband = 'bands.out' 
/

```
## Plotting the Results
We can extract the data from the `bands.x` calculation which is stored in `<output_file_name>.gnu`. Importing and plotting this in Matplotlib gives us

<p align = center> 
<img width="557" alt="Screen Shot 2022-03-22 at 4 47 05 PM" src="https://user-images.githubusercontent.com/76876169/159594949-2741e6cc-2065-4d4b-b97a-5db002acd15f.png">
</p> 