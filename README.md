# test__
open



HG



select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 from ((select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, mnt3 from ( (select :chap1 as lib1,:descp1 as lib2, round(SUM(case when :chap1 =substr(lib2,1,3) || '%'then mnt1 else 0 END), 2) as mnt1, ' ' as lib3, :chap3 as lib4, :descp3 as lib5,round(SUM(case when :chap3 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '010' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy'))
)  where mnt3 !=0)
UNION ALL
(select lib1 , lib2, mnt1, lib3, lib4, lib5, mnt2, Null as mnt3 from (
(select :chap2 as lib1,:descp2 as lib2, round(SUM(case when :chap2 =substr(lib2,1,3) || '%'then mnt1 else 0 END),2) as mnt1, ' ' as lib3, ' ' as lib4, ' ' as lib5, Null as mnt2, round(sum(mnt1),2) as mnt3 from scinv where idiv = '010' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy') )
)  where mnt3 !=0)) where  EXISTS (
        SELECT
            1
        from (select round(sum(mnt1),2) as mnt3 from scinv where idiv = '010' and (lib2 like :chap1 or lib2 like :chap2 or lib2 like :chap3) and dop = to_date(:datep, 'dd/MM/yyyy')) where mnt3 !=0
    );



SELECT

a.cha[1,1] cha1,a.cha[1,2] cha2,a.age,a.dev,c.tind,a.cha,b.lib,

            SUM(CASE WHEN a.sde<0 THEN a.sde ELSE 0 END) debit,

            SUM(CASE WHEN a.sde>0 THEN a.sde ELSE 0 END) credit,

            SUM(CASE WHEN a.sde<0 THEN a.sde*c.tind ELSE 0 END) debit_cv,

            SUM(CASE WHEN a.sde>0 THEN a.sde*c.tind ELSE 0 END) credit_cv  

FROM bkcom a,bkchap b,bktau c

WHERE a.cha=b.cha  AND a.dev=c.dev

            AND c.dco='25/10/2021'

            AND (c.tind<>0 OR c.courind<>0) 

            AND a.sde<>0 and b.cha between '100000' and '999999'

GROUP BY 1,2,3,4,5,6,7
