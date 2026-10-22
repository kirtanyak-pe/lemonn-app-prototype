# Flow: Stocks (`stocks`)

> One self-contained file for converting this flow to another platform. Read https://kirtanyak-pe.github.io/lemonn-app-prototype/CONVERT.md first. Design system: Lemonnade V3 @ `cd84910` (components, tokens and rules: https://kirtanyak-pe.github.io/lemonnade-v3-docs/llms.txt).

The Stocks tab — the home after login: indices, holdings, products, movers, watchlists, ideas.

Prototype: https://kirtanyak-pe.github.io/lemonn-app-prototype/#/stocks · machine-readable: https://kirtanyak-pe.github.io/lemonn-app-prototype/flows/stocks.json

## Screens

| Id | Screen | Figma | Taps go to | Note |
|---|---|---|---|---|
| `stocks-01` | Home | — | `search-01`, `profile-01` | Top of the page: search, indices, holdings, products, shortcuts. |
| `stocks-02` | Home · top movers & watchlists | — | `search-01`, `profile-01` | Same page, scrolled. |
| `stocks-03` | Home · top losers | — | `search-01`, `profile-01` | The "Top losers" chip picked. |
| `stocks-04` | Home · Smart Invest | — | `search-01`, `profile-01` | Same page, scrolled: managed strategies, then the announcement. |
| `stocks-05` | Home · sectors & highlights | — | `search-01`, `profile-01` | Same page, scrolled. |
| `stocks-06` | Home · highlights & idea baskets | — | `search-01`, `profile-01` | Same page, scrolled. (The live app no longer has Trade ideas here — removed 2026-10-22.) |
| `stocks-07` | Home · idea baskets & disclosure | — | `search-01`, `profile-01` | The end of the page: the disclosure, four lines and Read More. |

## Spec (README.md)


> The spec for this flow. A developer should be able to build it on any platform (Flutter, iOS, web) from this file,
> the screens in this folder and the live app — without reading the rest of the repo.

- **Prototype:** https://kirtanyak-pe.github.io/lemonn-app-prototype/#/stocks · one screen: `#/stocks-01`
- **Source:** screenshots of the live app's Stocks tab (dark theme), shared 2026-10-17 — there's no Figma for it yet.
  Rebuilt in L3.
- **Status:** 🟨 Home done (re-matched to the live app over ADB, 2026-10-22); search is its own flow (`search`), opened from the top bar (stock page and lists come next) · 2026-10-19
- **Entry:** the app's home: after login (returning user), after account setup ("Explore stocks first"), after the
  first deposit ("Start investing"); the Stocks tab of the main navbar. **Exit:** everything on the page opens its
  own screen (not built yet).

### Screens

One long, scrolling L1 page. The board shows it as frames scrolled to each part, like the screenshots.

| Id | Screen | What it shows / when |
|---|---|---|
| `stocks-01` | Home | Top bar (search, wallet, profile) · index strip · current holdings · product cards · Do more with Lemonn |
| `stocks-02` | Home · top movers & watchlists | Top movers (chips + two rows of stock cards) · watchlists |
| `stocks-03` | Home · top losers | The "Top losers" chip picked |
| `stocks-04` | Home · Smart Invest | Strategies on the brand tint · the announcement · sectors |
| `stocks-05` | Home · sectors & highlights | Sectors · Highlights (Mutual funds, F&O) |
| `stocks-06` | Home · highlights & idea baskets | Highlights (Mutual Funds, F&O) · Invest in Ideas |
| `stocks-07` | Home · idea baskets & disclosure | Invest in ideas (baskets) · the broker's regulatory line |

### Page structure (top to bottom)

1. **Top bar (sticky):** profile initials (left, opens Profile) · search ("Search stocks") · Wallet. Tapping the search field opens Search
   (`search-01`, the Search flow: Stocks tab, keyboard up); nothing is typed on Home. Back on Search returns here.
2. **Summary area** (grey, `surface/secondary`):
   - **Index strip** (scrolls sideways): NIFTY 50 / SENSEX / NIFTY BANK — value, day change; each opens its chart.
   - **Current holdings:** the value (the page's biggest number) and the overall return; the block opens Portfolio.
   - **Products:** Mutual funds (240+ funds) · Equity F&O (0 slippage) · Commodities (0 brokerage, "New").
3. **Feed** on a rounded sheet over the summary area (radius 32 at the top), sections 32 apart:
   - **Do more with Lemonn:** Trade ideas · IPO ("8 new") · Watchlist (count) · Refer & earn ("New").
   - **Top movers:** chips Top gainers / Top losers / Under ₹100 → two rows of stock cards (logo, symbol, price,
     change). View all.
   - **Watchlist:** the user's watchlists (name, stock count). New watchlist.
   - **Smart Invest** (brand tint): strategy cards — name, min. deposit, last week's daily results, AUM, slots left;
     footer: monthly charges (₹0, was ₹300) and Invest now. Tag "NSE/BSE approved".
   - **Announcements:** the campaign's artwork (one banner, the whole banner opens it — live: Jio Platforms IPO).
   - **Invest in sectors:** sector cards — stock count and the day's change. View all.
   - **Highlights:** Mutual funds (tax saving: up to ₹46,800 a year) · F&O (Nifty 50 today).
   - **Invest in ideas:** baskets — their stocks' logos, name, count.
   - **Disclosure:** the broker's registration line, four lines, then "Read More" opens it in place.
4. **Main navbar:** Stocks (selected) · Market · Portfolio · Mutual Fund · F&O.

### Data

- `src/shared/data/market.ts` — `indices[]` and `stocks` (symbol, name, price, day change %, initials, `logoUrl`).
  Shared with Market / F&O. Values are samples from the live app.
- `src/shared/data/portfolio.ts` — `holdings` (current value, return %) and `watchlists[]`. **Fictional** (the
  screenshots are a real account).
- `data.ts` — `products`, `shortcuts` (with badge count / text), `movers` (three lists), `strategies`,
  `announcement`, `sectors`, `highlights`, `tradeIdeas`, `ideaBaskets`, `disclosure`. Each is one API section.
- API: one home call (sections in order, each optional) + live prices for the index strip and the stock cards.

### Components (L3 → any platform)

| Where | L3 component (Figma name) | Props / variant |
|---|---|---|
| Top bar | Actionbar | `search` (placeholder "Search stocks") + 2 `ActionbarAction`s (Wallet, Profile) |
| Index strip, holdings, shortcuts | Card | `variant="flat"`, `padding="none"`, `onClick` (tap targets with no card look) |
| Changes (indices, holdings, stocks, sectors, ideas) | PriceChange | `sm` in cards, `lg` + `arrow` next to big numbers |
| Products, movers, sectors, baskets, highlights, ideas, strategies | Card | clickable (`onClick`); strategies `padding="none"` + `footerFilled`; highlights / ideas `footer` |
| Section titles | SectionHeader | View all / New watchlist = `action: view-all`; Smart Invest / Trade ideas = `tag` |
| Movers chips | Tabs | `appearance="pill"` (filter: hug + scroll), icon left |
| Stock / basket logos | Avatar | `src` = logo URL, falls back to `initials`; 40 (48 in ideas); baskets `shape="rounded"` |
| Watchlists | ListCell | `variant="card"`, `as="button"`, Avatar left, chevron right |
| Shortcut / highlight art | **Illustration slot** (`Spot`, src/shared/ui) | md 64, success / discover / brand tone |
| Counts / "New" | Badge (`count`) · Tag (`error`, `sm`) | over the shortcut's icon |
| Rows that scroll sideways | **Rail** (src/shared/ui) | `visible` = how many show (a fraction lets the next peek); `rows={2}` for movers |
| Buttons | Button | Invest now / Explore = tertiary · Details = secondary · Buy = `buy` · Explore now = `brand` |
| Main navbar | BottomNavbar (`MainNav`, src/shared/ui) | `value="stocks"` |
| Layout | Stack · Section · Inset · Grid (Figma auto-layout frames) | Page: Stack → summary Stack `gap="24"` on surface/secondary, feed Stack `gap="32"` on the rounded sheet; tinted bands Section `paddingBlock="24"`. Each block: Stack `as="section" gap="16"`, heading in an Inset `paddingInline="16"`; index strip a horizontal Stack `gap="16"`; shortcuts Inset 16 → Grid `columns={4} gap="00"`; strategy card body Section `padding="12" gap="12"`; trade-idea buttons Grid `columns={2} gap="12"`; disclosure Section `paddingInline="16" gap="04" align="start"`. Content inside clickable cards stays CSS (spans inside a button) |

### Rules & behaviour

- The top bar stays; everything else scrolls. Rails scroll sideways and snap; trade ideas snap to the centre.
- Index values and stock prices update live (prices tick; the change colour follows the sign — never by hand).
- Movers chips switch the list in place; the chip row and both card rows keep their scroll position per list.
- Badges: "8 new" (new IPOs), a count (watchlist stocks), "New" (referral offer) — from the API; hidden when empty.
- Smart Invest slots: show "x/y"; when 0 left, the CTA becomes "Join waitlist" (not drawn).
- Holdings: a user with no holdings sees "Start investing" instead of the value (not drawn).

### Differences from the live app

- **Top bar:** the live app puts the profile avatar on the left of the search field. The L3 Actionbar has no leading
  avatar, so Profile is the second action on the right, and the search field is L3's search mode.
- **Sections:** same order and content, rebuilt with L3 cards, tags and buttons; the grey summary area and the
  rounded feed sheet are kept (they read well in light and dark).
- **Smart Invest / Trade ideas tints** use the L3 brand and discover light surfaces (olive and navy in dark mode).
- **Shortcuts** use illustration slots (green) instead of the live app's icon artwork.
- **Announcement** is an L3 card with an illustration slot instead of the campaign image.
- **Sample data:** holdings and watchlists are fictional; tickers and prices are samples.

### L3 gaps

- **Rail / carousel:** no sideways-scrolling row in L3 — `Rail` (src/shared/ui) stands in.
- **Actionbar leading avatar** (profile on the left of the search field).
- **Illustrations** for shortcuts, highlights and campaigns — `Spot` slots stand in.

### Changelog

- 2026-10-21 — layout moved from CSS-module classes to the L3 layout blocks (Stack, Section, Inset, Grid); nothing moved (measured at 392). The home shortcuts sit edge to edge (gap 0).
- 2026-10-19 — the search field opens the Search flow (`search-01`).
- 2026-10-17 — first version: Home (7 frames of one page).

### Changelog (live-app match, 2026-10-22)

- Re-matched to the live app (Pixel 8 over ADB, dark theme): profile initials moved to the left of the search; Title Case
  labels as live ("Do More With Lemonn", "Top Movers", "Invest in Sectors" …); shortcuts Trade Ideas · IPO (5 New) ·
  Watchlist (3) · Referral (New); today's indices, top gainers (with the real company logos), Smart Invest numbers
  (92L, 356/500, last four days), sectors, highlights, the "Oil Price Dependents" basket; the announcement is the
  campaign's artwork; the disclosure has Read More; the Smart Invest tag alternates NSE/BSE Approved ↔ Auto Buy & Sell.
- Removed **Trade ideas** — the live app no longer shows it on the home (`stocks-06` now shows highlights & baskets).
- Kept fictional: holdings value, profile initials, watchlist names (the phone is a real account).
- Still L3, not live: the Actionbar's search field (live: a filled pill with a magnifier), PriceChange (live: ▲18.56%
  without "+"), logo sizes (L3 Avatar max 48; live ~60 / ~80), Spot discs on the shortcuts (live: outlined circles),
  the Mutual Funds highlight's badge artwork and the baskets' dark-blue card fill.
- The second idea basket only peeks in on the live app; its name ("High Growth Media") and stocks are a guess.



## Source — this flow

### `src/flows/stocks/assets/announcement-jio-ipo.jpg`

```tsx
���� JFIF   H H  �� LExif  MM *    �i            �       �      ؠ      q    �� q�" ��           	
�� �   } !1AQa"q2���#B��R��$3br�	
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz���������������������������������������������������������������������������        	
�� �  w !1AQaq"2�B����	#3R�br�
$4�%�&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz�������������������������������������������������������������������������� C 						�� C��  >��   ? ���o�u��˥h�g3N���?4��pp:��u����~���`m���v(�ny�@'� ����4��<`* �JG�<�a�s�{�`~��Ǉ�8���b��ky=�c���^l���U5e���
���B�jp���7Mt�Ĥu	ʎ����p@$W���(D *� 8 ����2<WEa�U8�-_�ݿ6�<�Ԕ��(�T���( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ���C�ε��I<[|�Rȷl�vJ@V>�9�\V�4�8Ƥ�����oc�袊�AEPEPEPEPEPEPEPEPEPElxF��:ͦ�o"E%��W�8'�ˁ�֏�|%��?U:f��܂H�PvH����+����^�W����s�������QRm��@�EK�R�b��sb�i�qK��d.b��mK�������e*m�m4s��m2Hc�6�U�
���`x ���X�h�i7}s%�o�~�b�wc�5��T,N����y����I��|g��{���iZ�8�L3�L3���3���������/�� ��6�e�5���� 4R���H۱�A�~M�>`3zS�e�T��iyI-.� �+�{��|d��-Q�eEt�.���~�@�P�m��i4G�J��?#��������:��8��{��G��j���������_���m�!n�B�۹��h�z� �)=9NH#H�;�2=��N�����3Ӗ��㌖�5�W��	d������9=d���?Z|�r
(����(�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (�� (��oxf�ź���lv	2�Ɍ��A�c��EgZ�)BU*;$�ƕ�=�G�m�x�t�5f���dt<�� a0s�~���7ǒx�U6�22�V�,)�;����Wy�o��z6����1"��'@�V/�?3���J�2�3�����-��=�Ysv\�(���30�������N�v��<�>��JI��(��zS(���j�v���[���AN9�������d�vf��6���/�F־Ԗ!�I[kT�؎���"��B Z��6����BHU$���m-�r8-LѺ1GR�����-%4�й�	%��. s��du��A������'��[H�
ŮY!e| ۆ1"���oʾ^	[���|7�[��� nGgC�!�#��s\��N��Hk����*,����t�*�}:�3��c�Ob?�q�T���|?a�[�@���U��_���;��8����
��m^Y��]v�%��f��fW
i�*p��mz �e(J�m.)sJ]�.�]��`!�K���h�J�C�R�6�6Qp��F�Sl�e�����F�.�_���"Լ1o�x�ڴ�r����� q�G�����+��źJk^���m������/��_?��?���_j�y��a/4W7����]=,.-B�?����UQB T`� v�Q_���aEPEP�Y�"�x u��^ʻ�+{O�[Z��@�r�� ���oκ*����L��7�i����G�"���� �&�r���.��nmt�����s�f��mKI��y�ͪZ�k'a"��:��={W�`x�*�⥁���V��Ԅ����j�t�|���-7��/��h� �Zo��_���qomqy<v��<�J�#�5.���UU�$��U��L��-��]9��0��n��D�!�#����f3���_pr�s� �����ɣ�i����^��6����kcr`��ɶ�D�&�;vF�öx��犷��P�SI�����Ā���:�����ԕ�}���)���M� ?� |�?���~��5�ɦj1XŪKk:Y��"�h�B�ʬ�m$`�����uXt�yl��w1�t�8��dY�O������)� :��7��.S����~��4�-7��/��k����>����i���y?i����q���1�v9ۜ���k�Zm�j��{kk&6�5���wtò��ny��9&���Ӧ��<��i����G�"���� �&��7Q�n>ɥ��y>��]�m+�N��w5MU���$��$�W���k�_��N3�i����G�"���� �&�RҵM�Z��s�LT8��'�ʒ@`�� ��Z^�U��^��څ���4��$j=K2��k?�Wd���m��)���M� ?� |�?���~��5��^��k�<Z5���ǂ�m�T�v�q�f�h��Կ������q�O%���ݏ+����9��9&�段{h����E�� ��� �M��M� ?� |���GÞ ����m2���b�}��H���B�'���S��4X�X�/,c���	"V<��<)G5n֨��m}���O�E�� ��� �M��M� ?� |����?���j��	$�N �'e`�X�jwIe�[�uq&vC4�6�X�E� I��3DsW$�����)�� �-7��/��h� �Zo��_��קX�cĺ�rM�i7�q�!��ieU�@%��2<�p�����h���{����m���6l�����Y��iM]o���p?��M� =���5�y�^�)}�b�{~�z�������Ӯ�<߹��d�w�w���dV��)�)))/발O#��=M[i����`��z�+٥QN*H���+@
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
����v�|7��$� P�LH���`� � v�^�o���iH������!a������܊��^"7���`-��꧃4�@���L������E,�/G�O�+e�f��9+{ys��Ox�I�$idcݜ�Ϊ�E}
I+#0���Լ}�K}O������}�(�H�1�`�3 $��_���t�_�u�aP�R!��e�]@��Xs�@]ƿ����0b8#"���)�\�?a���s���n��4tz9J)��QW9?����� �����R^�$~J�y���5̌9� ��O�k�*��?��?�Z��'�sv���QG
�;*� +d�U�%.�B��$�<  �M{�x]��^X�,F2��_RRrN�E~[��d��ɧ'k�&�1P��a��^��߅�6�x�s��K�N�F[3!H��A#Њ���_��0�Z��BjQ{4�Mט�Zveh����v�z(*�Bp�������0��w>�O�|]r-<5�]j2��F̠��6������~/��jZiQ�Qq6��Z��ˊ2�� k�F�j� v� ��iJ[#��{Tmo_���>+t��)����-�o�p��S���mm3YӯXtV��!�FC)r�Z���Z���T�~=�T��ھ�����s�����ۦwOg�������Z<R4r�GBC+GPA�k���i��������֍|�����ܩ�-�=��WX�R��*���'� �M{�q]ٓG2�� �Z��i���Og��z��ĊS� �L�隽㏅^:�{o����J��IK[���W����� �=�<�
�3��ę.+,�S��&����Vv�q�,��]��d���>�����]�O�O	*H�$��8 �<� �ʸϪ���Q_[~�?�n<�K�rt�~p�%��rTz�w�8�����#{��F��3I��^/ê�}+�3�./��/�k�1�U�I�\�
�� ��}mN��A'e��g��I]s#Ă�]�(JpJ��l����*�jsx^���ԁ��W��P+��׆¾%��@?gc��X���T�+��Im�K��$lx*��}���m㟇�~2�\]Y e
9 �I���`}k�q�Y�+����/�}����>h۱�&��[�����Z��&��LG=�>d�z2D>b=�+s�M�_�x��Z��
����9�2�\��@wQ�������0��� �}�\���~^��I9I�k�]���jt�p��[�~�����qϓAs� �%��sR���:�]H���?,��8�~���]�3&#ד�V�~����� ��a�yJ�;�sy8ů����6�?~��^:�?�g�|Uk.�a�mk˖Pw��b�z1`	�@:�W觃��.�U�p��Ӡ�n�M3�ܳg�n��h�-j )�s�?Z����� u��\|A�Va�I*�t���������=����6�����;S��_�DF
�x�~ Wɟ� d?��}ε��/��N%.,� G��L��#�ힵ� �������R���9�P}�W���7.���k�n����غ�x�ZH�s�-'�����6�h]��a��ᔎ��Ql��j?��'�� �x�"e�/&��.�M�oh#3�+�{�9�X�_�6kK0�R�Q�jqOG{6�i����� �'C�ڍ�ߥM��mzwdl��(���T�i6�vv�]J:�V ���U���� ������Z[iqR�{�?�����(��p����( ��(֐EU� �OV*���G�Y:=��Y�?��qߎ��Z���i����mg\��U��垡�ua�{H�	��&ޣ� ��S�G�"�Mե��Z�KCR�ߙ����@v��X��.��Z��q���Q������@�L;u�\�ܰ�u,��"wfH�-�I�ry8����q��l��}M��q��mճ��Yn�O�/v��Q��ρ��X�� a������|K�xF����<Kh/���\V���u��qk1�}�)`;��ϥ�z����j�T�k{g*Oo<g����{FElK�O�m�YK�\4#�n�h����$�u�a�L$%ǽ~��5<ΫniE�'�v�U7f��e�kN�*���<c�O	|�ޓv��|^7����ND��w�s�r#�A=��u��x�\���o	x�)n�)M���x���Z��G �������7�� �$Ӵ�%�����f[�2!+k*���H==+g_���?�:t�G�|M��Y�Đ�1(�s���o�q��-J������|�΢�<R_�uf����#�v��w�-�Χ�(|8�t��{��^�k��I%�DP=I W־9�j/�j���g2x{��7:a�Tk�u[3,�y����u������Ǟ1�Ӵ�&�V���D��v�
��ms������ϩ�m>!x��Ē���[��Z���^���s(����+����ҋ�^1�R�toޔ��}�et���ɍx��D��O����xo¿�� ��H�N���P��@g�v�W1��75�W�����>:���P�R���]Wú�mo���# 9۸�?{ ���29�˸�'v���s�ﵯ�� �G�/������}�YK!��G|c�ש�p��b���"�y$���q��N�o&��$��B��L���˟���o���_�z�k�/��&���Mik�gZ�[��/�{C�,�e`�OL�^1�x��ژ�|/�O�_h�����?�\�s]%�ů�7��z�爯����:]��'�%�fc63��c�S[�>:x�eL,�Z�aw{�ǟ[%g���t(�)F���{���xGB��$v�����H�r�­��I-�y�U��Ը��|q�G���Ϩ\�V�W�`>UV����n�<�FGES�_�x�����������5՞�b�Hʲ3)�
)A]�-����k��_B��i��d�Ih�q���E|�~ �T�a�\h�����)B\�\mi+�t�l�F��ݷ>��s>��� ������ƥ�^��5-"�]j0yo(�Q�a�F� �5�Z���~ѿ	u{�@^꺕���	-c��HV��p�� �s��E~u�S���f>�n������W�#�}�L� ����'�N�vu��)~d&u%
1��?]x�	�T�b+F�R��goy�T�Rv��},�Q��Z�����o����n�� �¯e?��U�/�"�zպ����r� �w\0T�D�����>"�¿x��Q�M.�Z�_����%͜�$J�aq�Ĭ�|�����|;�O��?�{Y�����E��L֭4��d8ˡ �9������/XG���_���"q"�<��8�r�Z��Lj`*'��]n����R��Z}d'YZK��^��O�~ʚW�_�:�ߊ����?{%����Y����j�/�7�N]C� M�5�Rx��xr/�3h�\���~�n
�2����#>���kº������t�B��sm�<�h�i�V*}�z��eS˱�**1u���VIM5��� ȏkyFO����kV~�#��N� �W��?܃}`�I�� >PS��s�Qx#�>�����<�����OAq����?��/�X ��繯�ž%�C����3�.�����ԗd30�� f��cT�uum�[+�f��GC��2|������*�/��|��h��%��ɻZ쿬l��>ڷ�<o�� �n�7��&�-��7Z�^]ȿi|P��� �R �Vo||']���$����o��r�T���.$,���@�z�+���5�NX�Isr�����e{ok��c*�R��m|��OPP���+�+��-��[$99����?�q��x�K^�(���QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE����8�Ok~4�UFH�#��|���́�����ƥ}q�]1y�eydcݜ�?ξ���_|%��7Vk���9?��=�����+�?�T�c�ە��c���'�Q
(�����/�s�?�$>*o^G��F�.z5�}�� 1b�~�^9:�¶2f�Eți������(�m���~|}fDE����0||�W@,}v���c����.&��K��i&��I�Y��Y��I���Ǆxg�O��׈��ς���x5�\�~Ҫ�I�����QoEb 2q_�?����i�|^�|Q��j��0�C�a��p�����oJ���?��%x�H�u�d�bY���L���@O>��ҏڣ�v?��
�$�]J�m���o�[���p�0
��+���3\Ej�8w/���>'�������5�����Kd|��L�f�⧊bѴp?��	���Ps���y�}��c�����ዥCu��>��jv�6���^J��c,�� ��^�	���P�a��߫�dJNr�G��j�+H������qC
�wc� �ھ�����/�?�h�f�O��D�by�D�	#�G� a�׭nj7�a/���~4j�8�"+kj����`�܅	d�� !_���h_�_������'�v��+ii9T�>���y=k�z��a�%O-���NΧڟ~N�����8�e,�������:��� 9������� �����z��b��4p''�rr;W�~3��� i�K+j^4���RI�N�k�	|��施��r����.���'�w�7�ͱU����;�����7�'�呎K���]G�?h?���f�� �u�FC���F�^?E{�ДyeM5�5^�wSy�;���
k�����m�?���MWK�����O@�W����'~��.�s�K�R�Ԋb���cʔb9��'�����R�<��%Ŵ���d��+A9z���g`�?�`[�Um(i��f���;�S�j>x�� ����y����^����+��P�N��0�ǯ��|��J���$��#e9{�^�� �ނ�-�~��&��߁ge�� � �bAxOތ�Y+ߊ�� i_���jq���;��)�>`�� e��a0�T�cn㎽}���T�K)�RU_�5��_����#:u��jN��>��G��-�P|)��ω�Q�ZƑ�/G���]��FO�T�_��9�^����S\�mՄ�2��/T�3�]pEzo��7��sa�h7=�o&�����@�c�(����+�/�������|]���co�$|��t�2?��g���sҖSQ��v�I� �b_���������������~f�M=���[;G,LNYNA�W�m�5�#��N���� �;�q��H?F����}�9x���*���;uXI�?�π�@�ߊ���q�X�oR�<���ri�M)��$ub� �ʹ���m�8��[>���[���RHأ�*�pA��B׳�r���#�<���7��U�j8�Tf�W�����p�U�y��M8�ݛ^�}c+���9+;���=N;��?	^�%��fuF��_��	]�5s���N��1(�_O._�����{y��:�㽮�V��r��>���<Ue���f��]vU�u���L� *�͛��9�$Io�z�w���� �l|�ܚ� �ϥ����uX2�m�$ܹ͇�$� �+��� ����}���)$�Qۨ�R!�]�0�u�#����5���<��������)Es�n�V�ﵚ�MK	��6���~�Q^uῊ��m�\�{W��tX�K���8oҺY�A
�ܡc�x�-\"��:��Z�տ3�S�WLܚ�+8��/�jdW0N��p���K���|͜t����yRGҵX˫Ԟsҋ*�� ����^u+I�ָ�$���U�.m�"3�J�ƽ^F
��N]<
�����u�Q_G$W��G6D� �PA���o�_�@��7U��-���čh�v��7��Ā��}����Cᯁ��8�W���D>X|�r�:�3_�?�&k�|L� ֱq��il��9�ў��X�&�r�#ͨ�~�(�t,�ie����[��c�Sq�ݞ_���6�]���2<�b�m���m��3!�Fڛi���qG2c�����(��p����( ��(ͭ��r��l�ЏC]e����@������WEcW
�g� 	��� ���i��� |�Ea�
^c�g� 	��� ���i��� |�EP����� �"�}_����-7�� ��(���Ù�� �$Zo�� �4�E�����\}B��s3�� ��M����H��W� �k����R�fw� �i��� |�� 	��� ��pQ�
^a��� �-7�� �?�"�}_����>�K�9��� �E�����G�$Zo�� �5�QG�)y�3;� �H��W� �h� ��M���
(��/0�g� 	��� ���i��� |�EP����� �"�}_����-7�� ��(���Ù���$zw�� �5�y�R�R�
��M�rtUGJ.�����v���,�rI�i�Q]b
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
��������^2.�����Y�b}��5�^�����ť�^|����� ���L}gGV��1o�C��H�?j"msM�ղ-m�V�L�����
�/��ѹ����ۖ&%��
n��5�ɑQT��_ʟ߯�UGy0���)���mn����9^�d$z�-^� �5��1.X��cqr3؝�d{���?��)��Fu�Sv�=YG�|�C� 'h���_�>�m4m����n��d^>H H��,�w�_W���Z��|M�͛+�Z[[��޾y�O�����
����p�<(�)%�ק��?�u۩��#�9=O�_�;���>���1��&�ʵ��	h�����т��=k䏋^:����X�LϺ	gh,��ZBvB�{z�޾���|$��c��;��n w}�Za��C�?=G�_j���\_W����q�g3���P}�w����󮯻��_�+�O�m�>|4�i?�%m�m-&���r[��d��k;��1߷ޯ�~xJ��^4�|!g�&�y�a���d� � f�+տ�?`Ь<'�8�QŶ�ik�������6��� ����5�T�W��ѕ�����N;� �OOé��*e^]6�?4>>�p�W�� ����O+*N��d������N,z�d��`(��;S�W�a�S�J4hǖ1VIvG�թ*�s��b����g� �w���	�V�qe�h�4�������;d؋��l�l�v������s���x����0��mX�."e*�I���͆}���0��Uw���S��k�j����|ڨ�*1�"����?���O�K���2��l�_>X#�K��3,�K3��s�k������/��!�xo�p�k��oiz֐� �����|�(�:8�b�{�x��c'��I�ɴ�T�^۝���xZJ��w>>��+�+��� ���~1�3P��>-��눬��Ÿ�m�\Y����|�o�����~!�[�/�|g����s5-��w �L	F�WVԑ^'d��p��{Mk	u�������,5U>�W��?�C������ڧ��L��K��H����h��.	�9��_��6>?�M�|5�Aq��-����Z���	�Y��]���S�'��hO(ho-m�켟��'�r?��F�b;W��ɞ#:/�d�\�e=��H��U��(�j񱘹g�-,DիQ��N������>�)S����L��ž���'�|9rKI�^Oh�F7y.Sv;d��g�u�jۙln#�GM��ه���� ��~/K}n�WZ���p�$�[�=ϒ��{���+�,���%�֣�
��4�jQ���wh��.I��>���t�uOi~!�P� e� 8��r}9eJ��(��#o�Ig�{��4zS��~k;O>�P�|S��������>�xC+�*�נ�������@��O��pG�	N	_؎F7g���k�:��c��#�E�A�?�$�x_G0���H�ۀ{ywx� �k琵����R��NQ�^� ��mZ}��K���S����1F�Ԋ���O�M	-?ė��o!u�ٳ^f��,N�V�S���O�3������O��B�V1�[h����?�c��߷ُqg?ʾq�K�W�.�[���� �Q��*� 3=�� ���Ǩ_���U�����X񟋼@���b��d�A�3�綊\
��e83��E/ЉT��e}��*�(�^�1�_ej��6� �Q�+��eX��qF��9�tW�K���K���a\�����(��p����( ��( ��r#��jY؅UQ�I� Ri6���V��>�kOscsK��$N�2p2H s^���m���F��v�zF�ˍ�L��*�S��OL{��qG�\=��x�6��Z	9�{�]��hݶ�(����;QE{ŏ��法n�����M�]<�:rA���|ƙ7d�3���a�_�I5�qi��MI4�[�j�EWԈ(�� (�a��.�vAA&� �iB�w>�v�w0H��^o��_~_��� �����)H*J� ��Pk��gßx�n�Ð�(�1���Ly���#��k� ����S4�10�B����U��^M%ݴ8Z+���u��q� �"���P��ٙ��r��Q�`s\,�-��j+�qjQ��Ѡ
(�� 
(�Mez�]��
I(��8��V�)�+�`U���o��<Q�Z��e���ck����!r��8����0�xc-��g��a���svW{%���M��i\�+�>(|��g�i���_=�	@�6y'ּ^��'�2N.�i�=���i�(�)F�2q��Qz4֨
(���AEPEPEPEPEPEPE���\D��������.�G����x�W���V���͢�Eiu=��`�I'�Z�g��@�犿� ��� @����'� YW̰�$�Z�bھ�KN���Ȣ��iZ��y�Vs£����~d
�[��үz3R]�M~ QW���SQ��;����Ƀ�1�4 �ך�WV�_{��y��Zz/���t^�,.�? VUF����a�)ǼZk�@QEt�QE QE QE QE QE QE QE QE QE QE QE W��	�M�Yd?���iԼi� �W�W��� ������� ��k���Yu{v.�ď=����ƺ䇌_���r��W%]/�� �p��'y� �޹��������"e�
���R�W�u��� �s�$,p}>A��>�����/�H��fs�xiƿ���էO�� ������)'�68nx�+�y�;�R�N��#�y*�� �לF�p+����~"��d��wG�y	� �W'����m<�1�,n��U�<w�]�Ҕ0��Ҥ���h%�A"[��e���#���?�)����Á�-�6=��ǽ~c�1��-��a�O�z,��q�q��ki��?3�5������ �
�YNM�������ׇSV��u�̡�D��3��\2�����������|@��� ����b�Ņ�g#��qP�ؤ@�M~��>�G������ǥF����_�f�{.��j��̷WS���-$���5��߶�|}IoN�$���y��p�ຶ��2�(5�������Ud?`��Rq�� k]s_1�\��le��t3����W�� �O_�4/�5_�;]��� �V�'��?� �� IZ��2��,_�}�� �}�W����G�o������N���-�=6��%���O�uoo*� ����m~M�h���	|P���Fin|Omt�.&���;�u�9���Aҿ���X�0�@���� �_!��� )���іҚ>� U��v�|RH�tm�g��^�qk�����ӄb�)���ֺ������rԨӍ6ۺZ��|[�C�8�����k>���E/�jZ�a��g�np=@ǽ}c/�s��Y��x�F���<��g����~�x�]�g�� �k���H��쌎��T��#Aܜ*�����૿o�M��xoU�Ҟo+�I��¡8��܌汇�.g*���)S�d�����o*���G/y�������1|_�q��˧��!�mO�j�?��+s�lg�k��k��O���w��:�:����� 2<S&RX۱ �V�_�ş]|0���o�ׄ�4N��Y��Q��� ��U��>
����LTj�Wm��n�{�No�,+S����߱V��?a?x����jmR��$.��-��;�J�����g�O
ݫ�Ωm7�3����!�5��H���A���M͸�E}��nc��� 1�#��&���m��e�l�u�c5ӑ(�����Ծu ���f�ܰ�y��o��S~ۚtb� �Z�_�{{�g8����+c񯅕x���m����㗓Q�>��c��_����ڤ�a���� ��X��K��}��{�&	n��~�7��� vE��3_�����n��e7w$�Rg�|q��p�7K����=|+N^\���_6֦U~�� ZpZ��r0�=� �K�τZ�ȃ��#�%�O�_>����/�Z������|���V�b��� CJ�݉)B��%(Q^�1���]�>�]����6R����ъ.��eO�1E�l��Sm��J�r�l���m4\.�6
]�6.(�\�����(��p����( ��( �����������|%r�|=� ���_��� Ҕ���w� �g� ^*� ����|*��EEa� ?��|G�]X��t���<1��pB���F��0Af�}����IO�?�?�Q|��)� �˭� ׌�4W�]�i��������N�zUy�%���,�%gi'&������G'����:׮t�|Y���1+Z�Hc#|�Gb	�p��k�4�/Q�.��K���w�JY��<t�<W�ߵj4���c@Y�;� �Ih�{��/��GÏD�$b��jm��۹�q��L���$�Kd�x)�ٶ*�F3�F�����+N3����o���R��ee�g����?�������g@����u��� ���^���^�U�8뵺��ڷߵ���v����l���ePGvX���<ױ�Z_�~)xI<Зv�;���2��� ��=Fx�~�(�v+��;��C�i)�R��u~�&�����Bm'���S�����<K�]xc����y����-�n
~W̸a�k��̿��ah�p�S�V1�d��d��^M4љ��n��z$Ws����Y�*G��
�[?�
�	�������}O?ʽF�	n�)5�
^It�DQԳB@���;�?�n��1 �;���rA?ʿÏ�υ�qv<�q�s�)М}����]K����7;5�����[b�L�G��h����������H�nT}�2�L��k�� d�D>(`��~�ӺtV�𕥾�p6�S\7[h�������_�����[r�,�o�M�h8�G�8�:���y&'<F	S�';�ϊ��c�v��!>O���a=��ߵ/����� ��~�|Srm<?a5�����W��8�5�/��\�_�M�f{�>��<��-���ɯ�|=�xs῅����k;L�7 .Qr��GRy?���/�B>x9�L��9�.��g)(���j1�������-�??o��K��7rh�H���D�̣�g�V���Ǟ �7�F�sq��W�2��A��}jj?	k}�i�b߷�y\��������4����(�!����	�H��M�|�w=��������.	��O��Z�
��t�.u�����jz���d��{�����Ɵ��x��isI�X�%Ͱۺ4��y��~��K�];� �u�r]\i������xQT�rX�+�-C��Ï��)�nl��n�-B#�*Y`}Đx�q_d��VMAԵ�#2��i=�F�#i
�z�W�K>;��5��ή
�R�*�wҫZ�p�*��6�a>H����~��N)j~f]|)��e=��։q�Ҙm��̒*4�F�#{
��q��������m��#q�Vy|���l��z��%�j������<k�^Ir�fRd[Ko�q�3?�}���Y5�M�#�ĺ��k9('�d
H�Fq\�L^1�;4Ἇ�E<9JS|���'R*6��4T�f�y�~��]��5�_�WI�e�5���`�Pcp;Ae$�ع�kC�޽�[���63^��DK�3�st�"����� |h�����Y�ڢ�6G98P8Pŏ���4�+¿
�"�PK[>2�r�Xc�LǠ�8�	��@�~�M����T3�K�����8ß�5%�*�����.iϙ��\�g�r|�����wl� ����k̵=+RѮ��U���t��T��Fz�:�+�ڃ�WZ��]X]Zٳ�l����02������ �5$Ac7����pH|nA�uG�c�\�/�ψ<!�`p.��p�\S�jӺpWI���#%�x�3�w��5ʞ���|'�/�-��,d��H��ʩ8�#�j�|7�Λ�ZiW�5�ww���X����$ 7���{�����^�U*�f��x �� �+�o���^�� ����G�cr��b~o- �gӎk���u�\�=nɲ�x�rC٨����R��R�M)4�P��j�d(��~jj�<e��z~��iΗڦ~� *]�8#�3�z�V�_������s�7��H�@RI%��� X���� �?�s��8�g1�C2yd��FOk�}U��-"�Z֙R��<��8�~]��b���x��u�K��y�CM�Թ��q��N�Jq�Ms�9B1�4f����j)��v� >&�[}�tY�������k@ּ;xt�r�[+���2=A�G���7�7�=O[�L������A]���2p�� *s�+����> ��}f�+���n i����9��31�ex����,?��1k�R��ڞ�ZN�HJͥ4�(�7�3�=�����!���Z�톏0��Q��{���J�|E�xL��$:l�K'�2�� \��g5�v��L�KJ�dҬ�./m���k��Uӵ�!���#"��H�9��jYV�Kխ�@q���Z�� K� �O��x�c��1R\����-_�s���>�$�+;(��ʞ����E�~�@��^i�D���Ye�j�M�?�+��e��3/�u��iww���f��#в�#���į��?��j�\��2�>\@��v�o}�!F�q�_�x����x{����\<N3Nc*���Q�A/z��m�I$����(�g�ǈ>���6�y�i��4��;���܌W����~5xSƚ<�j��q�R��H�]H�e�U<�J��⮗�i>9�`�̱K�JRx<�܈$@΀��_p��}/��9�w����̑౸h��K�M'�v乽��.3��n���4�G�������?<<J)&��}���O�� �/���z��_��ѹ5��M��ů�3T�gɿ��eR��_�q�'���e��L�W�䷚u�S{Ϙ��8�!���� ���� a_� @������/�x���Zw�l���-N)�4�"5�e-��ƿ�E�Y>w�� �<C��8:�4jϝC�<��y���[j����g�Wï]'M�9nYK{�B�����r'������O��q���ph^e�����ɝ���Xz�}j��<o���O��m��k��JC��$b����	?1�O�{/����|7�L��q�\��1ȳ1Ǡ	�}H��N��xw�E��]�O��:Q�QU�S��R2p�%�M*���}Cu��?�C�臊�P�����k~�id8fn�X#������⯇��⳽��LG�o��G�*3��܂�@E
b�D����R�"�~�i$����׵�KĚ�ֵ�Jf����v=NB�죠�{�ù��k�3l�?�jP��rQ�J���r�I;�K�<�f��mF)+r��?K4�� <u'�e��3H8��L3eq��׃�k�a�q��A佸2]�G�j2�D;���$s_[�Oi<wV�4SD��А�áW�oï����뗀4���.28i#f�S�Fe'�>*�q���L���#1�SZ��V�V�ݜ�%ʔe�3�*�n���>m�YEt�2Ңм[�h��V+��"������;����`ʳ*9�
�?�U�gI%%�30��+��(��(��(��(��(��(��(��(��(��(��s�����u_��m<]} �����u�|3�M;�z=ć
Әs�:4#�z��j~�Z�_�T���J��'�����re���$� ٫��k�>6X�����#��x&u|���'FSW�`�����	�& Z�w�[��|e����\�A��M�s_3^��'Y'Ľ(���{'������"�G�U3�x�J<�xy�.���/;�O2#/y|p���-v00������#��x�^W���ߴވ�~/�֐au+M�}d�m�� �,��l��G� �u���� �Ԡ���G�O� '��g�ǧ>0����1���F���N#��9������̸��c��|P��n|v�����i9 2�b1�ő � s5����k4��c�d�a�����8!<7��oJ�� �e��������5���O�N8�C�������G��I"�y���߈�����o\.�4�b�Ŕ�op�� ����#x�?	|pі��v��K��I����fH��_$� �D������׵�f���]�`8g�l���.Fǣ/�y�m��M9����K��_v�����		��� 3���e/~Ӛ���w��h����K/�(o"Q�Km%�a��O~�_�'��I/�/|��L�����#4��]$^6�l�޾�� �n����=�����p�e��%���Q��Lw�!�R>�pF����_��O�|�[-揮�װ�xn�q��8c�ƾ[���6�s:��*V\�O�����D`2�.'sZ]O#����?�g�x�@.����-�|��.$�x���e�Or+��+f�i�~����U���DPCd��Ҍ}~�x����/��d����S�Z%�1,�$�@�U�	bOA�6���s�G|Z��1C%���D�:E���Khرw�ı���8,�c���U�QNRn�M��/���K8�ҡ��z�/��1�Nh� �	�� �x��_��״��
��{�T�X�VV���u����=�p��+��?��� �G�6��.nl��⸻��,�Dv2��Q�~��?`�T���PY·wa�+��b�[x����e>��xC$�b18�,���Nq�M$�V���4�:U�����������/�_�_��)�H�}Nk{{�`���,x�w.��s�0zo��k�x[LЯ�դ�B-�O7�' 2�6���{�������9�@���U������/�m��drО�9�k�3��:�-�����{^o�C��ˑ��"�\wS����m�u{�mn��E�ܾ�>P��X��9�}O�
<#�a��h�5��������{�A��~�����Oڟ���0{u���r�2Y�Ek)� ���~�~�߷�ï����� �mJ��>.��Hmͱ��3�t����*�9#�:� :7��Z��Χ+Osw3�4�r�$�Y؟RI5�dظV��b��ά�V��۷m4<� �Rtㆦ��� #���	ad��?�S�� $u!�LivR�?>��W��ӛT���CnZ�2 ������F�+�χ�� � �zY�_�ٵ���Fxc&��
G]�`�v*Ep���׈uL�[t�������9�SF�W�P�gy�I˒>|��M|�z)8�� ~�l�HO��HI�����A������>د��k׾<����ⶽt��K��C�F�O�=�՛=�^N�_�p~XL�B[���^��8�U9�ɣ쿇��� f�#�6���	�*d
?�󯎂W�4Q��q�������D�o�9����忢�ԩ��]i�
�� ܢח�,����Ȍ-(Z�-(J�ȹ�s�4��:����G�Ϯ�X�+� ����G��5瓀}e��!^ �,�\�X�ޤ��cZ�ܫȀ-.ڲ#��o���`�ԡ*�J]��0\����m��S�"��_,U��l��
�]]Z�h�h���G�V�6J9���_,U�����G0�?�����(��p����( ��(Դ��_5�2�W�t� 6��1,O�F�n��|iqg�'@��B���Y�"��t���5�����'G�����#������H�|��vq��7)b���+�a�c��7�#��u6s.���F� N��'w���{��ϸS<�^�ܯ��a��5���9M�m8N-�	��Ӎ�Z(�F7�B?���(��v��� �e�� ��?�+����#i������㺻����m��a RG��� 9�7�S� ��[�����߇�%��E�&��ѕ?�U��ԕ��|4��JR��}դ�i�������A�|	5�<��f�=6	�-�½��E��׀<Co`��M��2���� w# W̟��� O��:�7�U��/��0�-��f�;}j�5�U�����(�	���r��N+��$��^pe8g^�]*�n��r�$һ�n3v��գ�E� g;{����Inn$�� <Kc?B���/~|4�՛X�M+#���G#,�����W�u�#¿4C
�-uf+M>3�Pʿq�n���?�#p�5�wp>S^�*�XT��c��K�<�����R\��S���cmY�/ǧ�O�:�BA�����ko�אU�WR��u;�Z��˛ɞy[��b��d�T+�I�� �*p� �9i�K��E��t���W����n����������	ǻ��|�'�[k��:��n�c����G�!�G��\�� �����C?�3���8���V���RR��_�{��i6�ǲ�A����6�&�9�^�y���1䎇3^��&� ��?��?�z�ξ����x����=Y�)�!�?3���0p�ЋþX+]�M�M��N��oE���ݗ>%I_�g�Z��k5��\�*��W��s����Y�E��4�22�Ss����a_4~�WS��Lүm\�5��m,n:����{�3_Q|9����D�#u�(�,%ݓ��91U?y8����p�s�����'�p��*^�+�.�m{'hԼ��k&��j��h�¯ү���֟4x�Ց��F��DO;�p{r=����3�|z�k�i�=��fR������.���6�>�4s�C#[���^�e\pB�8�Һ���J<����e<;�֍*UaV�J�IB|��i���>ny;��tO[������Jֹ� _P� ����/���� �� � I��>�7>*x����������|;���A,S-忕wj��:m�7^���W��'�x�7�aİ�ʦ	F�����,$��̩�&��wg�u~�|:� �}����2Wɟ�x?����<;�Oq| s$�"�1;��ʊ���������� I���?����w�28�4'���EFW�,�R����"�ς��4������F���*��b����-�n>^0�"���p���>9�T��|�w���X�ˋ;�>&�x�ܿ���/�D���D^s4^U�1�ˇP����;u���_�y�q�x���<FQ���UeU9oʦ�5�h�*������%~�|6������� V�=:�:���T���\���+ᮛ�.��ZD1c�FxU��Dg�OJ����G���q��1��W1�q��2ۆ�L�G¾�x��|z�HVK���֔�Q�s��\�.����8��9ɫ�$��K�Vy��9`�⏍��Ǔ#����v�q�U?��i<� [��	z��X�FH�����|M���lP� �QZ?����l���
*��['��/�ߙRè��|�	F���'��!� ����������?�3��)�B���q�����7�>&xs?������ h��%Z��v�� ҄�C�A��� >� R�G�g�~���� �P��>g�~���y9��_�����M?�� ��V���̯�����O��3_��۳|'��9��`}>�!����_������'��]� �L����I'���i�@���/�5]o@�ѭk^Gk�-JyTF��Rv�e �������/��7�������F��m:kv �1�=5�I�[o|}�5{��,�����������ʧ�+�?�W�4Q���7�ū �u2:��ž�����q�AU��è�yڬ?v�O���\���&�i9CV����E�~|K��P������"E=�����
��໏ ���óI�G� ��!�e	��>���w�<�o�ZE�Y�b�w�\]H %Ԁ0	�u��~4x����;���þ��(� ��,�� B����{_B�*�qu���&;���tۄ�	�Y^��J2w��JP\�)�"{Q_�� ��w��?�v��
�>�3��� ��}?�v������Ce?����P�|����N.o��� ������P�6�m�����Ov��Z�BG$��A��?�'���R�����={?�M� "�� a����"�r�$�r,⟴���ac8����x����8�� ڲCf�Lpg�+���_6���>"�Ϊ5mv໧ƿ,p�s�^��5��_��G� �|T��7�F�f����sI)8�ז��܆�?Y���ƾ������2:����>�qz��v��j>ծ�]Z#դ���pxe=�����~�i��Q
x���-�6�~�\啇_,��:��պׄ>�K�K��-�!���ǘ��7���� >x+��o�/f�G��+���)R�M_H�rI7h�r˖��$�qn6涹��{oo=�������+DA�f<  �M~��:���;�zF�vBKgm��'��V3J3��3�Y~�e���Lu[8m䋑ut��?���3�_�;	��x69����>���<q����>+���}$s\��8.�OF���Z�IE�Þ\�Q��%;G��nVI[Q.]Y�ǌ�X���Zͻ����x��弬�� �⹪(��*�h��*?�J�}"�W��(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
(��
�eq-����$$���0a���j@)I&��ݏ��:Z�{g�x��f9��.gU�!���+�p��݈_|�؍�zJ�d�����Bv����*��='<�ޔ�~W��u�O�*֎�{6����v�ZO��q�&��ES S��|Eu�N�X�FI���=~����㖟k�O���-�� ��7�e��pȾ�ܬ��W��y��>kv�)�� �u)�h�C��~�p���a�_$x���~��t���9Z=��t�=�H#�_şD�\�c�^c���]^U(_ya����t�R��T��$}I�x��� ]x>��˲�B����?�h����*��M�Y�)�*�\��f�� .�e\*�,��,<���|��j�F�m�i�476���ȼ�6�>�
�Vw�� i��H�bM]w�.����I2q��z��㊩��;���~�T�y>����S�����>�T~bY�\�]A}g#Eqm"K��#�C+������	�o�-i?�' ��^�K�����T.�js��g�u!@z�����P��u8^�I)cq�VS�����9��� e����/��F��xg]d��V�o��!}1������mj���`��|���_����܈r�2�Sih~-��}_V�]���0)�� ����*��� c���:��Ə��k�N�'ڮ��L�o���� �i	܄p��+�޻2���a����N��O�!��T�Tt��Z����2�i'�J���bj��Q^�Ih�W���؎�= S{b�)����%H�A���i>��I�&�t�������YtT���&�`�c�־����^��?4���LtԵـ�V���f����-�&�/�~�_���x+�V�Z�� �c=]�EE� �ߍB����</����� �����K��L�u��������x�8�B�p8=qt���O�[���W*�{i�Y������[�L%��m�i�]��<�_J� � �ŅzF� ~M������3�P��Ƈ�c�T�����߳�������{� n-,�L�dĖ��ȓv�c͞	4� �_�'�$~$��T����J���m��A�T\(� kux���u��8k�N��U�{����� #��^X�-ވ��If����ጒ�����fc�'ܓ]�í�i�{)1	D�v��_;~x��\DI_X���zf�u��Av5ʴp3�o̏� a�/�O�+�J|�9�a	Z�H�4�ը�c��y�A�](��#;����L��d�����NR0G��k�����[x�� Wc��R���>X� ��ֹ�����O�8*�j�U�9��ʍ�k��r�^QFu��6�U�KG����!��"Ƽwr �^��F:�� ��b�F�o��P}w
�c�T(N��)�!i$u_%KkmD���#�G���?�j���O�ݺ6�qmF#����c\0A\Y5GN2ݫ���V�sl�ӂU��ྕ�s��zӼ���a��.b�J]���yt��쭰Q��yt�)sʻh�V�Q��0���(�V�Q��`�Se.�󊵲�������s�����(��p����( ��(���#b��!���;�+c�Oye�Һ򳝾s�?�b�\x���qx�1�[sE;z]h��ػ���$��I�MMmywf��'�a�cvBG�*EW��*Q�8{9�8�kO�77���M���W�2;>3���@��xث)� ���i�R�B�8{(E(�J���Ox�8>̚�؈� ���u�4�\He���v��K1���EEr`�R�� 廌T[��W ��+�H��O,�\�#n�9��zVmV0�(���Q��I_� �V�׶[��q,��vL�v��d�Z*�ѧV�X�'ѫ��	�.���Kw4�8CH��q�O�[��ZH&���thث�sP�R���?cȹv������u��B-��;�bZV#�k>OR��ɶ��;c��rz�F�㡒��i:pЌ��b���+_�	&�k�iݤ���9,��O5%��ݡ-i<������DUz+�T)J���S�ֶ���Σ�^ ���i���Fp�O52k�H�E}r��*��� 8  x�h�]�pT��*��X$���5$�m ��G�Aё���G5�8Fqp��} �d�g��[ɪݴc��g��fv.�1�$��i�W+���XJ�}�b�[$�k˻6/i<�3�=	R)noo/
��圯
dv|g�7��Eo�Z���5���� ~�>)d�E�dt9VRA�ȫsꚝ�f������We8��8�4QSB��RtӒٴ�^��il�<��\��1�>1��m��(V����m����W��5;h�6�s��%uQ�O ��U_J�y+AIvi?��,�H�����,�I$��y5�a���YΝy=��r��~@�(��`���}^�8���k�z��sT]������ vI��N+"�)`�8|%5GJ0�����dZjڬ�0^\G𪒺�� p+>���j5�Z
K�'��<wW0����t�$���>[��9�<Է��x�;���@w�FpL�皧E)a(Jq�*i�l쮽@
(�� *�����������9F}pP���a�W��^
Q}M}�K�oY�oo�'0C����ˢ��.��MB=��_r ��+��(��(��(��(��(��(��(��(��(��(��h5 	� "�
�2�Cd��;\KMj�÷#tZ�y@z	!V$c��-���<���5���I!x2:�� 2}p	��4���*��R�m�[H���S�}�C_D|M�`���i@_�P2�[�'��>GГ_5]�S3�g�V\��Ko�hj��[��hZ�V�Z�V���;���,��^-��r~ϻɺQ�PI��w+�p+�� ���ZL>�q;�¾�7	��̲�uٞ�>������O�M�� `����K�s��Q�_ƿI^����	���e�M��+o����o�_?��E�i���+m{_��	�P�O�m��Z�����D�L�z���ſ��x'Yk�>2tk��c>SL,}��{�q^<R��8g��.;�9�[5W�����/�8=�%��R�9]n��?jMc�׈��'^�}�7���,/M�; ��H>	뎽+�:��jpr+����O�(h���e�\���ɟl��_��G�/��j�N�M�rL�i��N9��yW���5��� ��j�K�7�c��UүC\����+��?b��^�6
�����ׯ|-������}A���3XO�-����>�_���'��a�MBr���	��~k�˟��g�W]T~b���Nm�Vm�����&�E �`=Cڲ+�?�~&� ��4��~.x.�;�uT��X��@~h�P$\m�9"���� �L���Ť�|;�ee`3Gv��'���y��8z��Е���~��G�W ��	)/����W�E��3��.t߈vWo:�Vl}U���C� �L�^��3��?{�pE�sn"����U�"�����#�7nK|���_K����0�ԢOiRYh��O�_#EiIB@2�g3��E~��`���A}�L�.�m�r������J���d�G��5�B��7�L��b��g
�h���j�$~�"�|C�c� w�aZO�^T\�^inκ9,!�bg�_�z?�<=�+����hA<E��F,O;�7��2?�[��-�9��|g��O�'�W������A��H���^��>�J���U��P����v/$��wv<�I昫^� ��pr�&��J�����K����}Z˕S��V���ǿ�#��Ꮖr�$���8��i���8�9���k�$V�˹,I$�ԓ�4���ElY�Ms4v��d�V��%�� {�&I��������'��o�_�a��J������.�S����_0�Lgˉq��Ӡ�"�����_xj��:O�么Dq��x�����j�|?��/�流z�a����=y�}O=|��j�Ě�ֱvN�ܔS����A죊�<��?|J��A9d9$��g���.�%���a'�n���T�*P�����qs�{�y��'�5/�ҥ��3"�� D��	?�s��+ɼ=�ϯ���U�g���9v�5����J����|#Цht}Z�48]�a�[V8�@�-���.͡��/[���|��w`h9ɲ���A�>��D�Y�'�K�
�I�!���I�~�^�`�kshǻ u�H� 
��~��G�}��Þ6�y �6�'���pbW�`A�j/~�� �ׄ�� ��0��E.?Em���|/�D,g�k��� �s��̥n��i�����]f�q�%�� ~��[]�^ ֗N9���� ��8n��O�H�2�����)��B�]@#�z-��#em;T��aʀ츯s������;�O*�ّ�V�%~C��v�s����"�gˑ���9��M���mh_Aiz]��� |^�.8��J��~W���<���Zg�{E(Z��B���k��{L��X�f����U<������i�����%ͬ�rH� �{��q��_���N�s8�P�K㍋;h�V�R��9�s2��]�k`�`��+�v�m�{�r���]��k`�`��w?�����(��p����( ��(ꯃ��?xY|O�����T�[hU��e���`x��^_��.�@�sqnbK���˓#���?Z�?�-�xxf�O���X��?*��R`��S��Cl׊�N� ��|m���9RmZ3�b9�CD������s�k��e�cĔ18(F��R�I)�B��S��r�\ϙ�o{Ɲ�p4W��_�/��Og�����J#]6�e=&3��+G\���!��]�G������ z}p��Z���������a�]>WR6R����},�������)`���9#b��0��pA��G_�FJIJ.�(�]��?x�D��*ڛI��|ٶ6cb���{���������+m	e1�I�|F�O�q�#���n?� ���a���0���u"���Ԗ�R���|��Z+ܼC�=�B�m&�H!����������c���VF(�) �0AA�|�qfx��)�i:rR�oT�������]�����<=d�$��X����������1x���`���� �~a�����_?Ş5������
�^��Wۙ+�� ���PQl��Y�_x&�[x���Ò#�|�I����85�x?�ǌ�q�.��}���G����p���q��k�̼M�<MK����`�4�[�>�M��I6���ɠ���V�|;�xO\������vl� Fܿ:RpU��G����x�C������Vh��>�d��ԮG�*��x[%ʰ��m�ң�����JiB|�s�+��E9+n�7Et)�Ω��v���ʢ������"DY�`g��^��hV�!�R�Z]�1��lr�2J�=��ږw�Wd�F?�3:Tp��}�YM(O�.p�Ϛ	�[t�g�֖���z��z^�n�Wsn)y�)f�} &���&ҵ+�.�Mg<����/b��dp{������Jƍ������S��[,���N'�(�x|5Z�N���)����vW���%��k��g�mR�^�'��II$� \��x'�
�k�oڛ�D;��� �5�|_���Y����t��2�IT�Xsr���ͻ��������?����#�5����iDBv��),��X���W�|J�`���<9���rC# �"���9��_���E��iFyV(����F'8�RMy���ψ�?���a5��嗕%���&�U��|���Q�8��^6�r����|Y����U��S���|��^�٩*sm�i�^����ZEz�~x���Y���%�bh$� 1�$1��;��������P������ʐă�<�G�H��쇊�l�+�u�b�[.f�BI�����m���b,V���S��� ��m֫}����I�o�g	�c�����A����mA4�
�K˗�dc8�c�s^'���Y��d���(�gHizZ(鮯a٘�Wі��'���̸��#!��А8�<��ox/�k[�2��>d@����~#��p� �g���r���Z��ATW���I�E��g���\j�XZ!�{��ug����Ԑ+�u/��4�>�T��d���&�gܧj �7^�f�/	K���F��y���L��K>*��|7�#z�� }!_�_�}"<��Ȳ�5*��	r��Rr_��w�q[O�z���TQE_�QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE �) �J(�
�V�EL�P٘�� R*�m�*׿|���W��R��1�_��eđ� ����=Mx:�[��kY㹶s�0tu�)� ��^~e��/*��>ϣ���I�\�O\�K�-w�&8x����:z�@��&� |� E�����/������_6�m5��mp�9bb��0U���\�V>U��WҤ4��}T����J�j�Y�g����)b`���+)� ��V��
����5t�i��c&�|��X�O���kU�3j�G�$yy��,�c;�#�=�xL�ʵ J�.�߇�/����cJ�YV�y��;'ʛj1�IF6I!Jn[�y���]�L��_i8���@�!7��L�íy�p��53�o��SYxM�]��]H~��x� ��U������!�_xj���?�dڠ���#�7#،W�x'BM���o8	�X$���ZE�?BƼG�/�,<s�����ZbI�,���dt� ���~-n�3��)����ں�I\�R�P��i�>,���f��O%�̋h	��w�_jh֚�i�Ū�Kwq*N�؆�h-�x����xo��'�x5���T� e��b�	�$Q�!!W�f�������0��#S}�/w�^�fSU�
#Q�V\I��Z�\F_N*�+^I/yɭ�W�eћRQ�՟x� F��񶵦�ڱ�H�1�%>b�+�T��i]����*�5(���q��U:����/!���(W�����<���D�r(I�X��~8E{�G4�C8��#Z��ψ�5��>�%�/� �Wr��k�H���ڼ�� $�g9f+(��ʍxJ��m7��I5�����wGw��x��UU����T�OwoS��+�T��<
�-qp�	e<1����J
���b���ɽ[wm��9�>i��T�k��x=��Ioo##O�;���y=�}���b���J�G�S��;O���3~,�����7]����c϶+�٫�v?�/�o�#�[j���}|�,;�'�TiHú�+���:I,�	,��F��ee��:G���F� �\7�W����w�<i${���A���d,��ȗ��V�����8�JxE^���w�Q_�=̶��Ge�3�~8�5�TDP�� UP0 � �)��x�W�H�I����rz/��\�:�\[�{� ������X{H+�ҕh�ٝ����j�n�cmx�'�%�����W��=��u����X��)�IVLb�:�`M�g�ձ��k��A�l�O����^��
U �g�+� �d~���i4h�M
G�M��H����\��|���$��J���BLi�Zd������څeu�2�A �u\3LT4S=J�?��������,�*�� �����2pNM�s�����g��,O�HZ�W���©ee� T��W�m��~����:��wvw�<3�2��H�mde<A���6��֞��uh��Y�*�,��;�Ӯ&Ul�T��ۄ3�����e%�Tݚ9�t��IK���K�ՠ��k�/���b��G�j��6��̩�<�����s3)�ty^�om.��9��������(��p����( ��(���{�I������ �W�����S��Ŗ�����,��Y��&D>�����\������ �Z�|��z�Z�~8����Yu{I`X���ܾ���Y�s��׏�"�~�������nʟ5L:���e�^��4
>����]�7��[�B=�!=��3ܨ������|M���P�0�bt������|j񟄴�s�3÷)���񀻘�F���=1�W�h>%�w�n��;h����่	Q[ c�������7�qx.5�u��:���Z2�,���S�v�%H�w�O��q��c�/������k�2��^[#ܕR��!�X���R}�^A^������<X��Qo��B���rc;��y� �c��� b��d��&��b*V�{�s�oiʴQ��\з#���]̞������ $�M� ��_�=������Ox��5�X�wqj�n���"��"�B�I>��]O��� $�M� ��_�=���D���s����P���Süs�� ḛ��a�b�*m�7�PN\�6�����}�Q�_�o����i�����;�T�_�QԞv�`����>6x$��i�h�!>#ҁ�"�q3B��Wy����d�|� ��,���|W���㟁n.1�z%ˠ'�H5��9l|4�@qG�1q�%iܕ��Lc��SH��GK�sx�{�0xo���Qm�i�܀7������ܚ��?j�ë~�D�i��H�h	럻�½��f��|0��t��Y�p�#^K,�� �v� w#��'h�q�������\�>C��Sx�d���%(�O�{�O���?zW�4�B�kc�cQӼ9�3��)��ӵ[u�)07&�u�u?��|��5��h����j<���Tt�����l��^��{G�о�Zn��.R���d��*�ت�v"�C�� �!�h{�V��Y�����^;�Z%r}7����+�:��I�W�Yeg��a��a��L=i{9C�u�V�Ĭ7њ_�ށ%��,u;h��׳E���� �b�����t/AV����OM�`=I�X�5��C�R��Y������Z��S�@#i�>�$1��\wƟ�'�x.�d؇T�DE�c�pG�,���|�e��)��q����5\��֒��O��Y�E��m�1�R�f��ZV��uC`re�~I�+"����̱�7�Z��ChzB��G ���MW����$��_�;Y���#9�����*��{�?��
��blK�����$o��c�S#�W�nS�ǆ|�΍j����&�]�r�O����-��v�F,�K1<�O$��o���Jƍ������^-^��>�Xѿܻ� �Yk�c��%�$���+� LL�n}�Sȁ��b�&���k�oڛ�D;��� �5�|_������ ���)=���٫�I���s� �׎^x��/�^��W�kws��N�<($�+�s��c������ _�?�-|��đ�[_#�{C� ����� ��1��?�E�q���41t��^�n���F��f||���$_��%�q��"�C�/�Bg?P+�����_�|F���7[����q�p����`���_j|6�>;�{�jx��6����0b�p� o��¸߁�f�5��׈D�:��D���m�'��K��+�^�{�� ��m��e>Lm��%}o]�5�|��ʢk������W�_ؾM
�q��# u�<8yO�v)��O�?�V^�5�Љ�5XR��\|ؔo�<��z��ȿ����$��uk���Xǎ�Г1ǯ�J�Q_}�B�� 	�׶����@�D��J��w�s.���X^���Y�q6��p���^J�����/k��>i�/��u���ׇ$�O�B�$�M�`��(=@���������������=�ݝ���`2�A�����Ǉ,.����.�h�Fd���ƺ�7�-�e��x;U��q�9`V22u��,�g�`a�p*ʪS�yqOM���v���J_�Zj�ﻠ��>2�#�7�˝|�;�e���!�lтOR���+�_���_�B޶���|'��Rx��ך��K�J�Mo9Dh�{s^��C�N������e��>���3O	HNْC��9U8�Qx���_�8�
sz�IU�I�x�'(�l$�)4�ԭRW�j��BJ��4QE��Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@(��P���E 2�Cd6*�H T�3P؅U��ijeZ��lUZ�
 �j��G���ՓR�;���DxY���=z���#E�4[hҢ����2}�(�� }O��W�*��IL~N���;s��^N'/S���ӗ,���˳�^��Ō�U�V�U���AV�
)@�<Զ6ӂTʵ Jɻ��S������v�%�������Z������5�:N���/6��� �Q����Z�sLJ�a+b?�-��pW�G�?��@�}��Bb;'�,q��)��a��g�� �5O��N���[IR���80�H?�'
���5���3|��
��b�ԯ�@��d�8�m���E�=� ߐ�.�8�Z���"F� ���*����d�4U�b&��򎭝�u.�F��]?�^�����[�W�}��#��5� ��c+؍��?����>%6����,'�)��m��Q�:����������`�/Cr1!&9��?7�$���_<?�،hz����/*���9����rW�H�����TwS��M�v�������NiF���:���á�_T���[Ga�:	��q��� ;W�_���[�f��fK/&�8� �R� ��j��H��Ü_��7�$���~�y��[�+$=*�ES�`T���8"X�S�T��*�C��AjUJ�-H�ȓKA�F����`�\>��� 2���3��������2"�<��Un�#�ے}3^&���)*á��v�ı=I��q8%^�*U��y|���4�^X��g�|q�����r�Z���"������	?�F�>x�Ų�W�<@`��Zh¶}<ɤ����_ړR6��4�l�֘��m� ��J����|*����hSd���ڬ��:�ė1�� �N�{_�x��殩.�+��A���y����X��y��J����,����T'�o����@w��J�}?圄}E~YF�b��L�e�O���?e��� ��u�?�nͽʰ�`A���L����a};��t<ʼ�R�̏c�3��&Ӵ�l������'���(�6sr��ݚ�ec?V��t]*�Y�m�����ޑ��@����?��|]�|_z3+Gy}!� ����a޿�����m�ٟ�6��#��W1��l��,P�p�{���왤�ӼA��� [<�}<�i~>b��~a}�:2}� $��c�2�ˇ����J���	_м��ܭ�����b�ˣ�.U�(�*חI�9��]��=�J�����`������(��p����( ��(�٧ZѴ�  �A�_�[HuY�$�$m����5��b|7�����4��-�֬l%Cgr�e��K.{�|� E3��ѣ+˸�?�L^-׆iJ��%��ZTܗ2�oHr촓�W6�?M���7����iuqk4���wl�"8�p�r �M�i�>Z]\��i�(<�2����R�� +�Y��BT���Ӥ�iq�>?�I�u�f+�B*�U�qF&�YZ|��[�;4��:�����M������]�������L4�~�n�0d��<����`+Ǩ����'���Ȱ|;�EƆ
���V��)6�ޚ��!�������1���uKy�k���4h�31V`y��ś�{�� ���&������� �y���� G�/��y�T��<��n��������II�Y�j�r���}e�-�f�?��w���[<�=�3g��23�����;��]�W;o�Ep�&r9Rp{���Q��=aix�_�O��ԩOcȹW�cF����<�[y�6�?A>|v�<M���~(���X�Dnҝ�\1�[�'���=+��
��?� []!nw>Ѻ/��9�_�u)�r�Y��� wq��_�qG�C+��b��k崱����=\b���5��Ke����>�����D��ۍ�7+{�̦?>#����m���}�8�|�:H%V!�� �s�Ze����<;�OS+��:�J�Y�ΣI�{$�cw���z�ۖ�~�x[Ǿ��7�j�:���Z�,�<�$�G��K0�����8|E��ZJ��i���E$l�d�F��4W�}�_���x<t�s¤)ӔU%9��%&��ȴWM��]X�@��4-c�z6�}�ZGu%�K:�<h�tcː�fe���J�����L!���Xγ�iV�­�����2�$T��xxK�C�8�1<i��J�����t�U/k+�%&�,/���zlWV
���v�?���٣��V�t��D���O��W��p�8��s.�Uӎ.�Z.I]�T��̕��{����Gꇈ��ߊ�O��{o�e�K��$Ue����k�u|�J��� J�����`��|�sҿ<h����9)����J�e���E����k�B����?g]sD��r��څ�������d��v��W�n��~'�6r��m�6��#2#��W����-�u�,|�O�'I�%iR4�3n��n'+�Y~�~5��&�|-�]Go�o��gX�H��U�2�a��_Nx���|?�KY�Q�������xݤ��B���5�cE~e�Л��Ӎ�q�#0�(Օ9U�i�n
*^�2q�����ٷ-[�Jĳ�5��\�9�Y]�ݎK3�O�5�w�ύzv��G�������l��%cV%�Rc�d���q�_&Q_�^+xK�x�ó����WR��e*s�j2��I�ڳM8���%;�����x���r�*�f�b�ñ|7�|b�g�����g���"kgl��q��� �<b�7�y�R��ʧ��*������Nx�*�n#�c�xw���b������k5���u�f���=s�L&��T��P,26�4��sǧ��Z�#�U�e͌��c(�#xؗd"!�!�c���N.�B�%GA�~��ѻ�y�E��1���\�,)���%W���Q�I�m4���E�,HQE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE(�(� ��QR��U��R(� ���Tʴ�Z�V��[V��
�ECd��S*�*ԠVm���U�U��j j`��E<�m�R�ЫSY�Cb� � f�U�l��U��� d�����y��{h����dl}C?Z�1V�C� e��|<{��I�_� b:�X��^�'�q��'���ڏ�w�&m�W���[𭖹�}?Ğ!u�D𕬗�d��Vʖ�D�o#ԯ�|�SǷ��au�I�m�0����C��,r��}����w�*X����D�	&6��y	�8N;����޾"^We�Xxc�[r�v��߬�o��b��#�!���wƏ�7_���u�]6���V1�X�'��� d�_��[���>�����"kYe�$g�#of\�־�>�e����ڤ4i/���j5ye��s�B��O�*K+��.��43��$��u>�ă���ַxdh�R��U��8"�`|=�i�(�,��%��^D%LuR�y[��l��+��~�4O�Z�����Yc�L|���
���Z�����v�f���ݙ/v3G��Tʔ�Z�-~��y�
*@��J�R��["T�U*@� Z�Ȗ��ԁjUJ� �r%��?�~�]G�z/��A�H,�*/$�u)P�z������<����<=�(�4�-9B��- HF=�Z�j�'�O���H<�c�].�D���d��p?�������������E�e��-/ ��+Ꮀ��( ��;������~xR:æ.���Q�k����E������[��oi��g��6��������[��؁,Sm�)�nǎ��g���9�G�Γ���\��'+�r�_�9��srkn�^�<ܧ��R<V4?ٺ��_k�V���e����<1�$W�� �٤� g|+��+������'��J��X��^�� s�W���:�DO�"�Q�gO_=ⷀ�T��c|=ѿ���iDa���e~?塌3�ŉ�ۼ3�YJ��� �H�3ʖ��gQ��mY��.��w�1�U�F�j���e.`�*����V�Rl��.ʻh�=*ז(��>d������(��p����( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( ��( �TSTT�P�ت*u�Z�Ed��R��T�*c�je�U� �l�@�L�MQS�ͱ6("�"��ʵ�dU��R*ԀT6�R��*��+6�n��<���V��[V�U�V�U�� k���&����Lp��B�:a�_5����5�c�!���*Z��n?鬊�������r_	xW����<ֶ�]�/$���+�,���G�����z�M����݂Ӛo��>;�w���f�'�5��mVy�{��Yr����q�;W&<%�5����=� ��ekq��O¢��ŧx�L��� �u w�����8����T�֫q��6�mwl�]~L��=A�]9.�d�uiէ����}�b�v���i���	���8xS�m����{�)�¿�o]���_.*��� >��>1�^�̤�v}�g'��1���+�2�,^�>����}\��eN�<�c}��G��m���}*�g�&��Ԁ�Svӌ�g&�D���|@�RA�j6Q�8��� �Bײ�3�,~)����t[_��ŵ�[B�$�	�v�P�^��������;Hѵ�Nm�e�f��q��3��|>IN�i�n�֋z��;��Mo3����ϓ��j�jeJz�J�ar<Fƪԁi�2�f�Kdj�(ZxZ�R�r!�0���$��T�K�����MUz�N�)� ]eS� "��YB��8G�J+��?�N�!�w�a��$�ʺU�����n?������j��+�7�	�M�� |q*�ʇNҠlu�Z[��}��:����\�����}?�� S� �����O��<�gA���<��&ȴ���qr��v�3�ǁ��.��W�;�����m[˴�H.c#�~>p:�^2=*�p�g[<ΖN�w��������'R*\��}Y\į�^�U�k�x��E@�0�mĸ�b�{������+�W��xZ��^*�A+*G�q.>X�^� 95���?	�@��> 7��f���e)HHM�� ǽ�z�� ����9��p��	�Y�����.����5*[ݎ��m/�W�?�/Q������[sakq4FHHŒ�S�7Ť|�7�O�~|��O�n<u�M`k�#���C$Qy6ֶ���!�$�b>b�{���h���t�;+(�( �B�*�S޵�����̞?	��JJ�2P��T��u-�g�^�6(��՟�w�#To~���F�U�-�]@�/�ہ�7G>����0�(�j���U?�m���%�x�V���$[ٙ#�� �R1_u���x�.]��� ��������� U�K�ՠ��k��������omh�A�T�Fʷ�Q��d�?,Ѱս���(�f�����(��p����( ��(����~Ҽ#��->���6�g6�}
�,2I%�f'nX�7>��\|=���:xw���h�O��I��F�h��,�[J�"6���v���Q'�k>�ki��������j�;I$�*���ɱ���k/T���S���CI����dXX��!ԯ���7x����q��y&�¬�J��9^�MTj� X�+j~��/���&��K�O���f����#�4�
��4Y4�;��-�n��R[���^�������ό��u�;[;��u��� 	"��縐�n�*�ՉQ���E��/���n����=[@ҭ�R��2;c̖��\��0@#�L��=�,��ޟ���f��Z����ww���,��Ys&@l����vy�z،�<�&8�)ʬ#Z��mFE���Һ�ݭ-{�N��S����nkV�#Ѽcim�N�Ql�}��	$m�L�Gïqּ�P�y�xK����m*�?ū-��6̳���v7L�	��s�k��i� 
O��㈡��k�^x�D���X��8�?,�qɕ�i��1��;�xW⇀l_�7zռc©�g!� ��� ���� sY}c3��lUz�m�R����ֵ�d�;Ge}�Դl����#T��58b�,ti:׈4��F�W[�pđ�Î�8&�� ���#������.���k�wk�F(�B�R��='�>|H�o�� i�x�[�!��5)u�"�M�9���I0	èdT��x#���oxwź��n��)�ර����/���1��\4���!��x�*f���8���n�u;�*q��QJ�ҌV��KT�O%7�]���>�,<;�j֚�x�n�قQc�1}��E�Xc��G������<5}�kZ���f�Q��-� ��>f N?�ǻ~�ֺ5�����ӭ��F+[�FמKI��Y ܾ�W%�o�����x�_����� ji����mp���� ݸy�� ��k�	�fU��f�*���yF
��̠��$���v�v%ӊ����~��"��υ�-o��^�~0����{i<�̒���}��� uR����_�[N�{�(�h��M"�J�!;C2��y+ߡ�d�?�|o�o��<�oZxn����U�Q%�����B�W�{mA5�~|>��|b����.��ꓶ������	�M	|m(�fM��f�*�K�������9J	R���I���Þ)]im�4T���s�?��گ�|9y�k6~�-/OZ��n�b�F���v��O�>��8?eoK�'I�j�f�^��Q�5������ö�y^3с�^��� ���|S����7:ޛ��r=v� T�]Z�o��Q���!S�F��F
9�"��><x0|r��j�,�P�|7��I�}�--��1�GB��)@�8�:t���N�⢩ەT�y[�Q�_%�r��n�k�XJ�+-{9���`�4�
k�"�� �t���]w�6{����ZS��`��+����!��|]Ѵ/� �c$�m:��E��v�c��<aMo|���#�|$Ԗ�M{�Z����X���Q�h0\�8'׭y��_i����j�6��:�R�;�[�_S/�W�̰�j9�1���T[r�쭦��5��s/s�2H�������߈�7��OB�aM_�����IM�ŏ�`6?}*��xk��?��%ӭ�jI�5�cS��FP��ʹ�3ҹ��_���'��񕎣���ú���G�������n^d*	� qI�_������V�
��5��� C7�3�dm�|F.�>�y�����Zm�Z��E����۫7\���G�^>��m�4x�O�ͦ��/���Q��V�����eS;G�+�_x��W�� ��ᦑ�o �z�ê�Wv�FY����y�ɻ���gѼQ�{^��P�'V�%��h����ȸۍ�B6�c8��u��	i� �����V�cz�of��[�A�g�TC�Go�q^��R���5Sڸ��T�i�-�\��5kEy��]�Kymc�>���xZ��|m�K4��֢�aK31�0�NUB�����&����m����|5���"����9c����?!&�#h-0%�)�+����z���Z�e�?���ɣ�������	KE$��B8<n�A⽀�j�e{���*��#�Ӯ|w��}"�}�QBU�('s�����k�<e���!QK����{�o��mf�g�㍜R���u<?M��o5=o^�iCÞ���	��5�T`@Y�88=H�r+�|I�D�?죯��_�"k�~���W�NM��x�7΄:�
���p<a�M[��>��V�׼�T���&��T�A8�+�p[ W_�[�W�?f][�w�<_�5���/���H�G-�n�8�Qrs�5tc����p��M�V�������n��VW��e���*��ϯ�yG���þ:�<A��"x[��y�u$��I��ĸ���NIP;״�;T���8�᝿���ԒUҵ;Y�Me�e�3����o���w�<?���x�>�ӯ燙m�Y9��J��# ���?i?�	���Lo������M#K��H�i�C38
1��8�V�EC���GW��?W�|������ߟ��Ӗ�Nܪ��<�D���ilu�sƺ���4mV�E�u�y���V�E�� 2�c�Mz?����H�~>h�񶣦�Z=�q�ؘ�$:��i���3�a> x'��w�7��Ao�r�ė��S�F�Z�.�=�Ċs�k�=�8&�� ���'¯���<=��j�&��j:�ی&{�Im�@�ebB�O��9�q��z�=I�5T*���H���Ko'���Qj�Q�qkm:����X��� ^i֭u�M�I�Z���1�D6�$�aFH��W�W��]Ҽ;�x��_����v:��~.mQ�X�y�>C� %�B�@��1^e_�e3s�є���V�Vo�YY���?��Q^�!EPEPEPEPEPEPEPEPEPEPEPEPEPEPEPJ))�(�T�)�T�*3$QRM*�Ͱ��QMQS(�Cd1�T�)�3S(�lC�T�RR(��!��2�5V���
J�M�U��%�TT�RR��6Cb�ՅZDZ�Eg&KEN�H�S�Y6Cg���Z0��)�L�1ج׏� l�!O��ҿ��-<k�����z�
���=��/�/F/�k��/[�f�L�d|}<��֩~�����]EVȳ�{��\>�}����s��ҥҒ_�r��;��a[�|բ�ڎ��[��L�mwl��#���w���o������i�pz	@�Ll>��R�}�������I�_K�4��d�g�S���8��8���8�����?��O�[� +]T��ˇ�r�v�{� �k�����R�S��gM��i\	��9��7���u�*��ߋGŮ�����,����~�: ��M}�]cE���mWþ�����b��t��Lnz����e�{6����\۹I#q�V��ȧ,�,Vc�Z=�_����^"Ԡ�Og׿���;�� ��q�ꇅ����v�� ��5�G�/�S�֨ ��DwK� l�� �7W�z�08#�}���c��ᑈu����>�Æ��5�q��>7�/���M5��`=:��̵Z�V�x�F*�� �ӂ��]O!��BԡsNU��+7"*T�i�jEZ���������������6�Ԯ��:���a��Dj�R�4���Q��4%<�5�ۏO)V4��{~U��������}�~�V_|L����� ����\��G�|G��]�#��X����OĚ�������~YˣiO��W�Fw,����>c���?d�
��tO.A���J���u7��c�z����?�ߊ��MC�յB�mB;{ͯomq<�id\|�[;7gh8?����V���Ҍn���}"�E��9�Z�>]�I�9���~�>-~�772Au�KM:Bc�dJ_�eр�{ש�u����K� 	��F���5�YR���pШٳ��@8=��@��0 �R�}�yc㌡?gik�Q�^]�}z�iiT#�f~D�C�W�/�k�S�`���C�Qm���3?��C��d#��5����h��,�;�[%��a
�1�D�'�'�y��+�����:t�*0�i�H��Ͳ��P�
�~ x�<�?x��"�E��X���Kw�8<u��� �����㻀�fԭm���౾��	��4��
��=>z��v��O�L�Q����ֶ"6v���s�O�7�_�a+��ط?�5�}��_j~Bdgr[D�?M�0�}c5���п�_O�W����'���� #�3i�bd�hQ	N	V6҄�s��y�[�yb�l4l4s2*�b�,U�����`�E]�lge�(�d�����(��p����( ��( ��( ��( ��( ���~о �/�O�5#M�&���kMN2��Vb�ce��bN:d���ϴWa��Խ�.
Q��٦�O�8����)�X�7ŝj�U��� ��[X�[.�{hF>X��'���Q[a0�p�c���F�%�m����y�[� �^3��k�C�Ε+KSg�}�ѐ�y�ǥdx�]��G�5o]��>�}s}*&v+��Ҳ�y�,@�:(XJ*��r��(��6����X(���QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE QE ������&Lr��EF���d��QLQS���%�E(�*E�QS���֜���3��FOҧ�?���ѯ1�T��n,��:�z�sK���F�j����\��V���s�H֦�R����84rj*@)��Q]-���Z�
E ��V�"�j­g&@�:-1�(��%��Tȴ�Z��Y6A��2h�N�huL>�}<����@�ߝ|��U:���n@r�ҭ��<�To�|1���	�E�o��%��ȶ�dSO�Fd�� Ǚ��χ_�?��%��/�Z�i�����]��hOV9����7��������Z���z%��)�&�t�Gr��O�׾?��}�kM
�o�~�LhO>�tQ�]o�ϊVW��������"w�m8�� �c� <���c⥂؏�?B�h�cɞhx���U?��SՏ�|�_Y��V�֎c�������/�?��n:��8�:7�z^�uOj��֏1��݃)�;��E}a�i���E�,v~*���|������O�7c���k��u�kZ,�)�<��u�;��]Y�Y*�8�,�+C�}�YwO��ƍu�j��̋���l�bh'��I�2����>�gR�o���/c$֭� }�?u�^д��>*�¥��m�\�d�������?g���H��ޣC=��Ncq�V`c�z�k㸗�)��yjҒ��t��{�u�`�WV�2Z3�� �z_�O���%T]��=oޯ��Ȫ׹�{�>��(o�|���I�x��� �v׊*��dدo��W������'���-Y/1jUZUZ�V��9��R���U+7"F*�����0�Ӣ�.Ϳx����8��bs�H��~�*W�?%��_�w�i�Sn�1�U/���s���!P=s_�X�g���� $���Ij�]��1�}���ZiVK�����_��Q� +O�V����0�pO%s�+sX���I���m�Mo*Fza��|uueye#Ew��v��G"��O�U�8>xE�a#8��r�ԜU��V����ө��j8���
�Z�/m�!��8��ִ�d�_+2�J�PqZ���e� ����s�k�,��d�Y�Y�� I/���b{��+��?�>+� 5�Σ��A'�����`_�� �cb��y�FU����U�"u(��B�������
���~�S�0V��@n\g��O�}ˏO2x���P��R�YӠ���1N��8 ����+7�_R����@�'�^�% K��4x���گ�EFp�z8ʔq8y)BIJ-j�j�'t�[��y�/�CC�!��ė�M|���fwC� |������ � �EkxKDOx[G�B.�amh �<��?�TuE� O��� �����#ȔE�G�b��)K�2BR�8Z]���3��h�V6Ѷ�a\���⍕cmh���
6
���eat�����(��p����( ��( ��( ���/�&�:�ד�F�?�n��5���/���mr��O��+��# ο1���'���5�B{�7�Q�S�����o1�����?�����#[�-��F�*o�eO�j� ����+�	�9�!���j?J��3�+�O�eO�j� ����(� �T����� §�'���T� ���3�+�O�eO�j� ����(� �T����� ����aS� U� �C���_r�*xc��W� ��_�G�2��?�5� |E����C� C
��"�� "��n����S���� �"� 
?�<1� A�� ��/��'���T� ���g�tWܟ�ʞ� ��� ���Q� ���_� ���=>� �§��� ȇ#>���� �T����� �eO�j� ����(� ������?�E_�D9���'�2��?�5� |E��*xc��W� ��_�G�OO�?�0�� �*� �!�φ诹?�<1� A�� ��/��S���� �"� 
?�z|!� ��O�W� �F|7E}�� ���_� ���ʞ� ��� ���Q� ���*�����r3�+�O�eO�j� ����(� �T����� ����aS� U� �C���_r�*xc��W� ��_�G�2��?�5� |E����C� C
��"�� "��n����S���� �"� 
?�<1� A�� ��/��'���T� ���g�tWܟ�ʞ� ��� ���Q� ���_� ���=>� �§��� ȇ#>���� �T����� ±5/�M6�k������W^��"����(_���K��b�g��W������|&�s=��mS�5�� ��x~F�����0G���^4�x���?�������o'mb������(��QE QE QE QE QE QE QE QE QE QE QE R�Jr��T�)��QS""��
h�VLD�*�EN��l��
�E1EN��LG�R(��S(��lБEe��%S� �?�5�`+/]� �T� ��j�>��3�T�)�*e���p*�5EN�P��"��)�*tZɲX�Z�Fi�v��k)2$E��º;k�%ҴD������H�O�k�Q^��9��� t�n�O�{�����C�;�yY�/�*��匟ܴ*�y��g臈4��tk���4WP=�ȸ#u��sЅ'����*�R�l�������]"�y7�f#�"�����#�_lkz\Z���P>��)���S=?�WϾ>�u������{y������N@~_���~�-Ƭy躟j1��mi}~&�v��q��i[��-XU����o�� �I���� ����~?�%Z� ��M~��|���e� �G��ҧ� ?�Ϛj­}(>4�$� ̗k� ��H>4x+��-����y�?��e� �G��ti���3��)�c��:΍)�X�Oܑ;����ܞ	��ώ� ���[�]Jی��e,;�W�~��k�m�4xn�t���d2�)
��Y��� ����iZ����4�_��ay�?uN�d�N��<W��V*S�:u���[���k�g~
1�iN�^['�A�]+L��op�1�˻��:�eV����j�ǃu"���D�퓆c� |�U�w��<�j��-��_����j��
�*�*�juZ���8j�N�J�R��nD7qk�G��ۿ~2�z��#��:f�ڝ���C�}��E��T�F+��H���H�-�I���/�� �͏Ż8�}"H�|Ce��1�Yϕ.=	;[�}+�x���R��/��z�UuB���g�??�|_��+��-�oF��4L�2�C,N�**G��k��o��� ���-Cp+\ČA�ae*���������/
�,Z��oJ����6�c���^s5���̾|S�J>��h�c�85�7��T�f�O�W_s>�5c%����?�<��.b������!��z��?�f�ٞ{rza�c�������'xM��s�:���&�P=b+�
� �@�j?4�U���0WS�;��ܰ�}����n�.�9}�בE���F��?�+���*3c~��̊A?��vO��)IDj����Ĝ~<q_�~� ��|J�/�OK�׼��%����OҾ���s���"_xsWј�HИ��q�vg�k����µ��KR��*6��s~:0g闅�_�G�Km,���]�rX�ۜ
�x�jMK����u��|�[kJ� 8��8��8��Lq_f�f� ����÷?���CZ�����Q�A�e#���1���@�^+��E����p���Kx�dwz��ܤ����4J]�Ws#������7��|���0���~	u܌UEN��d~�m�OS_��?�?���mrz�� N��� ����/}�Y�f�jM��kҹ�G���.�6�̀�m&ڛmh�@C��o��I���o��@�����(��p����( ��( �v�K�[Q���_T/g���2cpA�c�nſ.k��U�	�!x�(�K�{*� @�������_��z]�u�K��,qF�
�� 
��c}(�F<)���kG�u4~��oi=��>���ˌnf��Z��MЬ㴁0��c��ԓ�5�E�1f�+����U�J�m�RnR�{���~��EWQ@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@@ �2��� t?E&��$zv�2۔m�s� 	����ۀ|D�ͩ�<9������/�v�_Tך��M\���4�GAԧ�5h����I�Gpz��Vm~��}�c��'�6��M��˰Q����!�e+�����_�������-�U<M7�^�K^���M{�~�m��+0��+�QQ@Q@Q@Q@Q@Q@Q@Q@Q@Q@J��jP(�2��EN�� x��V燴�}JI��%b��%����iC���5�WN��e{���y⊔W������7��je�ޗ��� ��s�Ɨ�<���2������� ��q����_Kl�*�`�B��T�p�ډVG�(��Tj*`+�[4fk��%S� �?�5�QY��� �T� ��j��G�����*@)�*E�9"����AVVRdQVTb�AS����lz
��Q�Յ�dQ_f���){�����`�����G����|l��Hf�7�x�0������x��ߝ|�x�c�T��n1�n� ά�<���x��S��Y���9� @�k���^�L~x�Me�?�֫b<#��m`#H7�����~c�p��c��Q�f)m:� ��ca� }5��1Z�Y�K��W��N�8��������%�{�Fx���f�,�y0�ti~&#�I���Ө���l�U�pA ���^"��}:S-��3��I�A��[�W��~�O�{	<A��#�5�w\Y�B�=�(���>��q����Y��ҧU�5� �}�J�++��]���)Eu��~��J=7E�i���#�u˶08��Ҽ�7�:������t�)J\�!�'����?�<s]w��*h�ӛ�� !G-���P�I� h��3;u*}[-J����6��$g:K����f�/�~ك�k>(a��?�� �ƾ{�?���j'Q֮F��G���ri����ndie��;��1=I&��Z�2�a����*��� $�/$e_��GH����ҧ�|%�r5+ߟY���$�x^)Z�B��>�������Y�-��z��XW;6�� ��]�[�~��(������a��8[�����z|�_�:���*uNUV�U�QS*���G��*ՅZjUZɲ[;!�"� ��F�Uj�������#Z�k��?�K՝1z!�s�\�6��O��M.��OQ$߮3] Zx�+=������?�w��^�i��彴�����[$�O�޼Z��'���+S�ӝ(2�/9�����!��'�� BOC��֓���FR�5{� �q�W��Sg����	x�݋� ��^�<m~,	�x���&�n��l�WP��d�>�v�������4�Gt3�����n�o��{ķ�ūخ�i�y��2�U�XН��"�P~�(���?��t.�-�ӏ��͌olt���W�����p�T�9�Y�Z�Z.Ȉ-r��� �I� � �EvAErz�� N��� ����r�ߡ�U��K���.��y��tE��mI��mb�"�I����I����"�F��m��wG�����(��p����( ��(�?�t�z�b��L]kNn]�Q
���� ��
���4=4�Nң[;H �8�� Jׯ���S�k�_�C���"�伡{B>���W�7J�(����Q@Q@V^��Xx{E���IVzm��w�(P��� �ҧ:��:j��$�����&��Z��L���_Xū\�&��ھYmێF2kr��}cW��5}S���Ik�X�\�$C{1������0= �;����[c�h�2ӱ�}b��\�a�xϺ6T��_�����?����Kƥ���U��F�t�[=65�K��YE~`��
�j�����^C�ֶ-,�a;̦A�6�Q��{W_���㻿񭿂�,�lxjKɒ��A�v���0RH��e���0��T��_�\�)����n1R���[�n����Wş�'�[{�7�^�����ӯt�]B�����-��H#U ���Rq^1���^:��o������w7>z<�NT��d�1���rd�
�fi����ᓧZ<�n�#�䓒|�m��)5t~����~�F�u�^ǧiv^_�s.���,I�����Tq��~񇆼w���_	_ǩi�"Eq���9Gp�t��~?�I��ş���~5�˱����I"��M�h$�L��{׏|5��⏁� �W��[������ڮ�i{糩��u+B�a+e$�+�^�Y�.'��Y��q�\x_g'�J\���\�/z�D���;�;���W�x�Ɖ��V?n-l�M���Aȷ{eo�&O?#����Z�/ٛ���O�˝v��A�Ҵ�$Es��;�q),#��>X�[�}k�WgU�1����*��哓��M���e{n��z��������[�R�8
e��b@OA��=��X���v�{���wo&vM���Ӄ�RA���/��̿���RA�Ұ�� ]k�؏�}�5�x�G�I�:4��,W2�T��g9Ra �s_k��I���/�8��)NIҶ������v�w��O��q�xs#�v]{B�Q]mF�;��[V�1;n����⵫��;�� ���yCy�_��=+�Ϗߵ���^OxgKo���Ku-�@e�6	�;W���q�����{ש��ƻVQPO{ɻr�=�m�k��z[��%���_۟Z�|Mm��f���r�P�1,��+	9�d�^��+ּw�Lj�����M���Ξֺ���������@���qc<�6/�u0˙ӝX�8���7,�jM]{���)^��4W����ay�3źo��}"�U���_\�u���ƣa�-�ϵv?i>������F_5��m�vU�嘕 ��B�H�?�<EZ�[Z���6����k����Q{9i�}MEr^�5_x+C��h�ڥ���֨IXZxĞ_��T{�[_��OZt*|Pm;;��gf�~�0��+ 
(��
(���Ad5�{�s©�� j�T	���6ð�q�(�S��+��"��t�]�uP>k�I�'�A a� �Ms~��1��x��E/�c��.=9�'V2�Q�D��Ț��B�(����(�� (�� (�� (�� (�� (�� (�� (�� (�� (�� rԢ��*�dȑEN*%0��I��;�^� ���]�q>�w� l��j�����Ǘ��-l=EL*5(��k���� i� ��ף^w��Z����ٗ����=5EJ)�R
�!³��� �� ��֚���� �?�� Ct��cꈖǞ��E^�4�u'uF�$���n������� �M{5q4��d�0�l�PT�+�_L?�� �&�O�� =���5�,m�'��9�2
���2� �u� �MJ|5:�d�X����k'�����`���ST�*�e&J��[����?��-��ȶ:\R��F1��� �ū���3x��:>���������$��O��_�� ua�|2��kCk8��>��_�x�7���ˣ�����͞�Z�cR��˝F�MOR���� Yw<����9s���E1Ew�'�|s�-ܲ)t��Q)����9���֥���RJ1����$�+-Y���]_��ge�<�#E��O@ ��|��L�[k�� ^�k�N�k(��'��}�>�z�=��>�Ic��<Q"��0DH�G� � ����|I�o�ڛ���O+��z"ʋ�_+:���8Q�<;�O��]"��΋C���n����⏃�+[�x?�m ]K��(�h`>�����5�;�e���}�}�O����1�#��?�����^�����v��G����1� �$Jx���z}+/��[u2��O�So�������mG�g�"��+�|!���� �����QӦ�#[���:������^�1GYN#�^�3���=������C���v��O����mWKc�Z+��������6>G���^�V�?�2T��j�����-���[�; ���|e��t�@`��'�U��J�I� ���T_��4v�x/O��>yU�
���2���!�UjuZDZ�Ee&fvJ� ��� � Ѝj��g�#�/���kL
�+?~^��� �(��lw��~�?�d� �� �B��k����kCR_�?�?���� �� �Z�� g���I���i�i�R�f��)�S����[Q_��?�?���-s�� ��� � �Et�%�C�CI4�P�0$�9�O�ȴ����k���\�O�,{� ��?���ȳ��~f��������`�{��ڕ!A��s[}���\6+�RR�31�eb��6� �T�b���������(��p����( ��(���.�a���&�dR=?�X�1�9��'Ý�6�m�Sz�-�wϹP��קW��q���3�~G�V��Jo�q�mu�(����
(��
(��
������� ����ͤ�/�Wr�@���[��	�z�R�h���3x㏊4x�Z������a�ape�%f, ڍ��F+�<3��%���ڡF\��rnQW�����},]6��g�n���_����7��5֣3�6��I����$2���3�#>��?���	��j��d�φ���uc���I|���&}7���,l�E�;x�TPp ��|����a���ωڷğ���[�k.���X�ԁ��9�+�L��^�.Ͳ�N�ը��<�V����/uZ�W�cIT���������Z��Px�kkK�7��'�eD	L��»(�d;�O��]���<s�M;�R�����G~�ed'Β/�DY3�I���¼��G����'��=��tz|�WvͶX�p���{�{W�?
� b��_<O���n�Cg ��^E*���T|�:��5��qGp�8�"�b��]
j*���7*^��{(�Z������z�Q��������0X�o�1�*����Α�����_����@j���� �~�~� )�i�vZ��$ZDI1��,ZyQ/̦7*ǁ��W�?�f�g�~�?��F15��O��K��#���"����	���}]na׵XtŐ���l��!�G����1<4�.�g�ʔ%F�T!̧i�㣼et��k6K�\�����]F�O�}��e���t�m�����V_l�}A��g���M����B��.�.�";������ �J�߅}⟁~����SL�4]X��W�
ҧ��c�����������C��M�=�;/iW�_Z��q"�rdc<�3d �����Y��2ɱs����K0X�q���FM�<��WW�ȉTM;w?t������x��5�sE{�G���H�!>���o1����h�f>|��=q��]G���q��U����ؿ�5����
h_�l�cV�m���?S}Z��s-�H��ur��Y��B�Ϥ'Ч����J>ʦ'���j��c�4��]�c�-��>9��?��o� �%�� ������\��d_x����Y\�	��.mV�g�G%������'3�7�&�>�{>�o=�{`� 06� pFz��e�+�>�|���ym���$�8y^RX.W#���<9�l2�����q����+��84��J��^�w&59b����߅���� ��|Q����&�����g����ظPXz�_�7�?�?o~��;G �Sԍ��x��Z��7 *���Ҿ�����	��a��^YK����di�VR>u�����q]W��ً���눵mc��u�c.�g�Hѩʤ��k��y�Nͼ_�Vc=:XJ�'���_5	96�_j1NϗG��F��on�>R� ���������3�i�V���W�NZ��G�������<�� ~���7�wL��w��$:χ,4��#�������<���� y�蟇߰��ox�����%��E�ov��gU>88l���k�Ýⷁ5?kR=����:�H^VTt�e{q_;��? ���U�֝|>��V���xռg�WeI��ҷr}����~O�:_�5?�?��=�G��E����Tg+so^Lk� ]<��ū��|Cq������/�M�ս�3ͼ�A�� �V��=��[�W��Ꮑ:V��h׷�֥�K���0�"B� Tns���p��c�| ����J�/��!��+{{��G���P%P���k�#�_���᜽�0����%��ͧ�������k|�>��4�(�* 
�8  �E�t��g(QE QE QE W�?���5׆mA���B=�0�� ��:�:� h����{HtZL�t�d���O����������\u5��)V�'�)StW����3��z(�����( ��( ��( ��( ��( ��( ��( ��( ��( ��Qր$Z�j1S(��rU*��EJ������� ��� l� �j�Z�!����� ���-|�?���~E�bU�SEH���
D�W�k��&�� �?��DQ�y�� !i� ���n]�G��FS���R
`�y�{�(��pĦ����������O�  � �֪��cꈖ������?���D��T����� ٫�AO�y|�"b��E8��T�+Γ*�*t��zVr&G�(�VPT(*+��'�￳V�5�:l���Ӣ��a��FcB~��k�OڻW�'���!P���;tf?��%p� ���׉<I.���1�њI}�'�^� ���:��k�mj�]Lh�H,�w��F�oBN�<f��ʚ�T�����Wg%��_��a�?�����ÿ���"��A�y?�4[}��7���6��j��#�W[���V�a'��@�V@l��!��c��|�j��+���<��:ݎ��� \�~c���������� �G���Q�<eE��k��X�R\������NN��_��� �q�$��ӻI#���I$�䚕���y�a�E�� �f�/��������'�����G�?�g&�� ��|�O�]� ߳V�>2��� �f��?� ?c��)� +���ֻ��u��Z?��rLRq���k�.t_�d����]4�$e�� #��ч>������k����i�����\�{a��[��H�k��{0���?���ƝU��Z�I_TtR�H�I��=�����[�^.�]f�����6`� �[��}	�����P2�*�=������_��N]�6�47�߅����[����\�'�O{Y�ΎHLM&:�]���_%�������������~�����mNj?�ש��������q�BT�ppjdZ��|���QMQS(���#�҇�_�/�֘�����_��0+����z�Qz!@�iT�+�(jK��'�� BOC��֯�C�O���*��?���tE��^��d�4n�N4� Z�lѱ�j@��)�T�KcB�-���l���W[\Ơ����[���k|'M�K�xZpZ�lԋ�5(Z]�����������mw`������o��M�1]��G�����(��p����( ��(�o���^ץ�&�-]����c�Q�נ�|�P���~:�20t%YNAGB}��_�u�����ϊ�X5h@��8[�������Z� ,~��F�n'?�j��̒�S���*���ծT�R�YOg6���LQE�X�Q@Q@Q@Q@�s�{�7��R���tϫ�1����)���׻QEz��u�ͱs�fU�Z���'v�_rI����??g�|G����Hך�j�Z�l"��x��{��/�C�j�j(��c��(a�U��Er�7�Sե�\op��gMMgH���H��[f��*,�P�0����֕�S�*sS��Z����=��SS�o3V1}�[����I�bq�^�Eߛg��<vcZUj�ה�۲I]�$��m݅Q^h��(��(��(��(��(��(��+Ğ'�<'�I��+mo8���� u�1�ۗe���O��*�j5�)�Rod�շ�7�O�x��z��S2���"y�vE�[�����������̓�H���;��OԚ�_�_u����$>�)gm����7��s�W�����F�~p���_�8�YU����I>��nmh�ڻQ�1��QEZQE QE QE QE QE QE QE QE QE ��ր$2��T�P�3%*�b�ZɃ;��� ��5v�\O������ ��ۨ���� _/ȨlH*U�GJ���d�^}��Z�����K^��Z����ۗ�~��{�*E�
�Ey�	�Vn��  �� ��֢���?�?�� CZt?�TL�fg��|� �?���U�3�>���]�я�4�_��~L��QQ(��W��*��z�H*u�Y��{�d��
�G5�28��� �{A[��y,xm^���Ԣ������~%|k���oJ�/R+���<�l@G��r�}���>|2�m.>UӴ���ل^l���-_���캞�u�O̗s�;� �#?��ƸCG4ͳmx)ǚ���V��(����ѣNvg��Ǐ�g�����¦~$������Z�2���Yw��� _�x�W���{*�r��z�+� ~��*a���7�W��/�W� ��VO$�� ��
� "^*������� �A��� �L>5�C?�_����^D��A�c,�/� �x� �+����o�y����~���¦_�������_�&QS���`?��~��D<]o�y������� ߵ� 
��x�V�G�Ě��IVg�B�!����J���ן��t��c�G�*��k����F���OD�?S�����M�y� �����;�@��@=����U�?m<�n+��xF�BA�1\��r���J}�<�L9j�y�(��Tj*`+�L�l������� B5��������� B5�����Y��� ��+������I� � ЅU��w����@�I� � ЅV���������K���d� x�е N��@8
 �J����E�$� �� �"��+���2O���+|+�ߡ���BӱKJq��n Z]���tE�\^��v��]�)|G=w�[m&�󊱶�����`�����(��p����( ��( �+20t%YNAGB6�M&���+�_�O�|3zv����)���}�ǽ}?�|w�q����~�+�;�(s��?Z�֢��|M�xq�����BX,D�r��c'��jP�w����?Za�.<ZŉS�q�l*o�J|1� A{�	�� ���
+�9�͌���qT����>�~E{O#�����^�� b� �� ������ ����������)��� �GS� G� ��}��� �J|1� A{�	�� �����^�� b� ��B�?�O�u?�D�`{G��o� ������ �����?�)����?�&/�*�$(��)��� �GS� G� ��}��� �J|1� A{�	�� �����^�� b� ��B�?�O�u?�D�`{G��o� ������ �����?�)����?�&/�*�$(��)��� �GS� G� ��}��� �J|1� A{�	�� �����^�� b� ��B�?�O�u?�D�`{G��o� ������ �����?�)����?�&/�*�$(��)��� �GS� G� ��}��� �J|1� A{�	�� �����^�� b� ��B�?�O�u?�D�`{G��o� ������ �����?�)����?�&/�*�$(��)��� �GS� G� ��}��� �J|1� A{�	�� �����^�� b� ��B�?�O�u?�D�`{G��o� ������ �����?�)����?�&/�*�$(��)��� �GS� G� ��}��� �J|1� A{�	�� �����^�� b� ��B�?�O�u?�D�`{G��o� ������ �����?�)����?�&/�*�$(��)��� �GS� G� ��}����J|1� A{�	�� ��MK�_���/}�Y��A!?@����+��6�~S�����4���M/���>��o�;������{��i��>����/�|m�?_�� ]�����8��8��_��}����d+�-gZ����OH'�AE>�!ɰ��+�1Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@H�H�"Յ��Z͙���qP��k6'��xG�^����]����z� ��5v�_7��<�_�qؑjQQ-J+�d�-y��?�m?�� @Z��?�?�-?�� @Z�˿��?ȉ�zP��2ך�e��o�AS� �?�1Zb�5��M�  � ��:?ď�&[3?�>���]���A� ?��� f��{Q��4���q���ED*a^xɖ�J�zV������mgM+�U5���m�=�+J�|����]��J���o�^EO�\�S���� �*o�w�k�?g/�>!_��n��i�Cm�EqrY��x��Cٜ��s����ׯ��c&�[�O�΍	J�b���7㞰4��!�F��Z}�1�7L ���¿'��I?j{}j� ���j�w~�J�#�6=�����1t����������R�R�NroV�VK�L�͹�Z�l�]EN�����1�  �������^/� �\������2����~�x����aEN�ԯ��_� @ɿ�����.� �d�����/�� ����2�?�q̯J�t��_� �6o�w�j�xş��� � �X�7���޿̏e?�g6��A]*����� � �<�� 0���񬥎��������U?���:����ټH��s�~ o�d/�|U� @����¾)�����(L�������f��~#WR�����EҍHԌ�^��~%���6�`s�	��Q^>�����u�����(uQ����xj�3�� ��� � ��{0��NITJ��^�FeB~יE�qEJ+b��z�	����(�V >��d^�*�k�5�yR�N�Ga����_��U�����_��Q^%w�����Zp���ʚ�� B��� �
������V���'�� BWD� ����[E��^��s�����i*E��*ԀRO�`�r�����B�P3\��� L��� ���
���a]��H�NN�r6Xд�i�iv��=��m��mq�����z�
�h�NV�oֻ�s���C�Z�z��t�N���`�7ƭ��t�:
ۯ��<T�߃�D��=΍/��~�ٜn��\�=������|��S��A�qW�ZI|�h���囈QE��Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@J�J�	�H�:�+S�f�$:�+S-f'��xG�^����]����z� ��5v�_5��<�_�qؕjQ֢Z�u�I���\� ��� ��kЖ��\� ��� ��k�.�+�� "'��"�Z�T�^l�qY������ ��b��fk���� ��)��l�� ���g� �Wf���S�^���]�u��y|�#(�L�ju�P�Z�k��N��	.��5ȮYR�X�@[�;���*��V�fB	R:��vb(J�v��.Y)W��_�>$��7��6���+z��;�r=9���Ե�����7������7��o��~ޔ�N����	�sW̢~��C�E��u�����
�a��� z`\ ��Q�?��o����VQ�?��o����Ya����F�m��*f\��'�R-|4���� ?S��� ���� �S��� �\/����2��_�}��"��k�_� ������XMB� �~f� ���k)p|����� �����}��jq_��� ?3�m�5e/���_���?�B_���� �L�����z��Z��o����_����__�Ŀ���e.����?�������>�*E������~%� ���j�_^� �Ŀ���f�N_���� �C̗��'��Z�bU�����r1�ҹ!_!�����K� }����{{� ?� �m\��7�_���_�K��������U�����]@��c�C �M|�)�qq(�,��ј���V��,��d�����>٭-c�҇�_�/�֠��+�<b� ��F�+���%��
*U�TV+j?��'�� BSD� ����W5���� �
������ke�	z� ���4o��TTj*a\25)�RS�Vm���_���?�?���Es���2O���+|/��j�	���
 ��\F�K�R�`T�q�k���4�va%�͈{ĝr��� <O�K����]�a��m��	$� I�s_������ �I���}'� ���~����lj�1��tR3�j�$��H�ˍG�|���i_ļR�xϫ�W�W�n�}�>�'ɾ������������1���|	�K}z�L���qv��9�88�+��� �|Q���h���g:�Q�Ue<k���čo��� \��G�l�b� Ĥgc��lpG�φ~#<����n��w��� �.����Z��1Xoh����SEy߃>(xK���ݤ {K�T�3���;w/�ϢW��_�aq�c���S�٧ty���AEWi!EPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPEPR�ER�{�N���Y�	V�Z�ju�����?��� l� �j�ָ�� ����� ٫�Z��w������īR��ԣ�pN�������� ��^��������� ��]�w��ꈞǤ��jS-y�0��o�As�?�1Zb�5����  � ��:_ć��e
� ����� ٫�N��xW�^���]�u���<�_��v'^�2�+֦Z����XZ�;U��ٙ悬%WZ����,�XN�]j�V23,-YJ��a+�V�%WZ����{��ZJ��j�V��aj�Ue�	XȂ�Ք�U���e"��S�@�:�/bY(�V�*�r ������� ����ǌ_�/�֘���%�u�d8T�P��k�ڏ�yI� � ЅU�zM� ��kQ� �)?�?���/I��?ֶ_������ЭJ*��p��}H�H������\���}��� A�-s����'�� Aх�ߡ�_�:�ҤZ�t�����0���Fi�⡒�+ƾ �� <)}�?�%��`��ۗ��܊$��9�TrO�Mq� �k/�#�������~]L�s|[	T��3�G�ҿ?h�?�_�&���=�U�F��[3{}�4�[�&a�H@�
8�4�j}9BJ�E������`rJ���R\���������o�ĭK�Z�c�Bm��S��M:'co��qy8i�� �����y֩*��'v��iS�8(AY#���袊 +�/��.i� ��3_�����?�4� ��?��� �������?0�'CEW���Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@Q@H;Tu �@2U�U֬-ffH:���j֦_�P��;_	���'�5ݯJ�<%������v�+�� �"�"� Z�u�W��íy�/��<�?�)q��� �Ez� Z��c�B���Wn]�Iz~�������}N�_q>��Z�"qҲ�����O�V��YZ��������(��X��'�(xW���O�]���'¿~�Z헭� �?�b�����^�2מ2qҥ?���D:T��S}�����XJ�;U���gej�Uu�	X�̜v�I�UQڭ'QYH�X^�a?«�Z���X��E��VS���֬�AX��{�Յ��]�X_�XH��� Z��Y�YJ�Dȝ�L�
� Z�k�,�T�Q
�k7����?��V�u�+'I� �5���a^o�^���p�R�D:ԫXH�u/��o�� 1U4O�/�Z��Ǔ}W����}�~�����9_�ЭL;T+S��#bAҜ��Ҝ���D�ڹ������?���{W9{� r}G�Ӆ���[�:����
�u�����*Qָ���ڼ���� �'�O��� �룵y�� $OƟ��� �+��
���6�� ��W(����8(�� ��
```

### `src/flows/stocks/components/Discover.tsx`

```tsx
import { useState } from 'react'
import { Avatar } from '@l3/components/Avatar'
import { Badge } from '@l3/components/Badge'
import { Card } from '@l3/components/Card'
import { Grid } from '@l3/components/Grid'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { ListCell } from '@l3/components/ListCell'
import { PriceChange } from '@l3/components/PriceChange'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { Tabs } from '@l3/components/Tabs'
import { Tag } from '@l3/components/Tag'
import { msChevronRight, msCurrencyRupee, msTrendingDown, msTrendingUp } from '@l3/icons/material'
import { inr } from '../../../shared/data/format'
import { watchlists } from '../../../shared/data/portfolio'
import { Rail } from '../../../shared/ui/Rail'
import { Spot } from '../../../shared/ui/Spot'
import { movers, shortcuts, type MoverList } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** "Do more with Lemonn": four feature shortcuts, with a count or "New" where there's something to see. */
export function Shortcuts({ onOpen }: { onOpen?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('shortcuts.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('shortcuts.title')} /></Inset>
      <Inset paddingInline="16" paddingBlock="00">
        <Grid columns={4} gap="00">
          {shortcuts.map((s) => (
            <Card key={s.id} variant="flat" padding="none" onClick={() => onOpen?.(s.id)}>
              <span className={styles.shortcut}>
                <span className={styles.shortcutIcon}>
                  <Spot icon={s.icon} tone="success" size="md" />
                  {s.badge?.count !== undefined && (
                    <span className={styles.overIcon}><Badge count={s.badge.count} color="neutral" label={t('shortcuts.watchlistBadge', { count: s.badge.count })} /></span>
                  )}
                  {s.badge?.text && <span className={styles.overIcon}><Tag color="error" variant="primary" size="sm">{s.badge.text}</Tag></span>}
                </span>
                <span className={styles.shortcutLabel}>{s.label}</span>
              </span>
            </Card>
          ))}
        </Grid>
      </Inset>
    </Stack>
  )
}

const moverTabs: { value: MoverList; label: 'movers.gainers' | 'movers.losers' | 'movers.under100'; icon: string }[] = [
  { value: 'gainers', label: 'movers.gainers', icon: msTrendingUp },
  { value: 'losers', label: 'movers.losers', icon: msTrendingDown },
  { value: 'under-100', label: 'movers.under100', icon: msCurrencyRupee },
]

/** Top movers: gainers / losers / under ₹100 (chips), as two rows of stock cards that scroll together. */
export function TopMovers({ initial = 'gainers', onStock, onViewAll }: { initial?: MoverList; onStock?: (symbol: string) => void; onViewAll?: () => void }) {
  const [list, setList] = useState<MoverList>(initial)
  return (
    <Stack as="section" gap="16" aria-label={t('movers.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('movers.title')} action={{ type: 'view-all', onClick: () => onViewAll?.() }} /></Inset>
      <Inset paddingInline="16" paddingBlock="00">
        <Tabs
          aria-label={t('movers.tabsLabel')}
          appearance="pill"
          items={moverTabs.map((tab) => ({ value: tab.value, label: t(tab.label), iconLeft: <Icon icon={tab.icon} size={16} /> }))}
          value={list}
          onChange={(v) => setList(v as MoverList)}
        />
      </Inset>
      <Rail label={t(moverTabs.find((m) => m.value === list)!.label)} visible={2.15} rows={2}>
        {movers[list].map((stock) => (
          <Card key={stock.symbol} onClick={() => onStock?.(stock.symbol)}>
            <span className={styles.cardStack}>
              <Avatar initials={stock.initials} src={stock.logoUrl} size={48} label={stock.name} />
              <span className={styles.cardSymbol}>{stock.symbol}</span>
              <span className={styles.cardPrice}>{inr(stock.price)}</span>
              <PriceChange value={stock.changePct} size="sm" arrow />
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** The user's watchlists, each opening its list. "New watchlist" creates one. */
export function Watchlists({ onOpen, onCreate }: { onOpen?: (id: string) => void; onCreate?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('watchlists.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('watchlists.title')} action={{ type: 'view-all', label: t('watchlists.new'), onClick: () => onCreate?.() }} /></Inset>
      <Rail label={t('watchlists.railLabel')} visible={1.45}>
        {watchlists.map((w) => (
          <ListCell
            key={w.id}
            variant="card"
            as="button"
            iconLeft={<Avatar initials={w.name.slice(0, 1)} size={40} />}
            label={w.name}
            description={t(w.count === 1 ? 'watchlists.one' : 'watchlists.many', { count: w.count })}
            iconRight={<Icon icon={msChevronRight} />}
            onClick={() => onOpen?.(w.id)}
          />
        ))}
      </Rail>
    </Stack>
  )
}
```

### `src/flows/stocks/components/Home.module.css`

```css
/*
 * Stocks home — the insides of each section (the page layout is screens/Home.module.css). Tokens only. A section is
 * a Stack (heading → content 16, DESIGN_SYSTEM 2.3) with the heading in an Inset (the 16 page margin). Most of what
 * is left here sits inside clickable Cards, which are buttons: only spans go there, and the layout blocks render
 * block elements only.
 */

/* ---- Index strip: NIFTY 50 23,146.15 +0.65% › | SENSEX … — scrolls sideways (padding inside the scroller) ---- */
.indexStrip {
  position: relative; /* keeps absolutely placed bits inside the scrolling strip (see Rail) */
  overflow-x: auto;
  padding-inline: var(--l3-spacing-16);
  scrollbar-width: none;
}

.indexStrip::-webkit-scrollbar {
  display: none;
}

/* One index (divider + card): a horizontal Stack. It keeps its full width in the scrolling strip — a Stack sets
   min-width: 0, which would let it shrink. */
.indexItem {
  flex: none;
}

.index {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-08);
  white-space: nowrap;
}

.indexName {
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
}

.indexValue {
  font: var(--l3-text-label-14);
}

.divider {
  flex: none;
  align-self: stretch;
  border-left: 1px solid var(--l3-border-light);
}

/* ---- Holdings: the biggest number on the page ---- */
.holdings {
  display: flex;
  align-items: center;
  gap: var(--l3-spacing-12);
  padding-inline: var(--l3-spacing-16);
}

.holdingsText {
  display: flex;
  flex: 1;
  flex-direction: column;
  gap: var(--l3-spacing-04);
  min-width: 0;
}

.holdingsLabel {
  font: var(--l3-text-description-14);
  color: var(--l3-content-secondary);
}

.holdingsValue {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: var(--l3-spacing-08);
  font: var(--l3-text-heading-28);
}

/* The round "→" that says the whole block opens Portfolio. */
.roundArrow {
  display: grid;
  flex: none;
  place-items: center;
  width: var(--l3-size-40);
  height: var(--l3-size-40);
  border-radius: var(--l3-radius-full);
  background: var(--l3-surface-primary);
  box-shadow: inset 0 0 0 1px var(--l3-border-light), var(--l3-shadow-elevation-low);
}

/* ---- Small cards (products, movers, sectors) ---- */
.cardStack {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
}

.cardRow {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--l3-spacing-08);
}

.cardTitle {
  overflow: hidden;
  font: var(--l3-text-label-16);
  text-overflow: ellipsis;
  white-space: nowrap;
}

.productTitle {
  overflow: hidden;
  font: var(--l3-text-label-14);
  text-overflow: ellipsis;
  white-space: nowrap;
}

.cardSymbol {
  overflow: hidden;
  font: var(--l3-text-label-14);
  color: var(--l3-content-secondary);
  text-overflow: ellipsis;
  white-space: nowrap;
}

.cardPrice {
  font: var(--l3-text-label-16);
}

.cardMeta {
  font: var(--l3-text-description-12);
  color: var(--l3-content-secondary);
}

.cardRow > .cardMeta {
  white-space: nowrap;
}

.highlight {
  font: var(--l3-text-label-16);
  color: var(--l3-content-accent-success-default);
}

.chevron {
  display: inline-flex;
  flex: none;
  color: var(--l3-content-secondary);
}

/* "New" sits on the card's top edge, at the content's left (the products row leaves room above for it). */
.onEdge {
  position: absolute;
  top: calc(-1 * (var(--l3-spacing-12) + var(--l3-spacing-10)));
  left: 0;
}

.onEdgeRoom {
  padding-top: var(--l3-spacing-12);
}

/* ---- Do more with Lemonn: four shortcuts (an Inset around a 4-column Grid) ---- */
.shortcut {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--l3-spacing-08);
  text-align: center;
}

.shortcutIcon {
  position: relative;
  display: inline-flex;
}

/* The count / "New" over the icon's top edge. */
.overIcon {
  position: absolute;
  top: calc(-1 * var(--l3-spacing-08));
  left: 50%;
  transform: translateX(-50%);
  white-space: nowrap;
}

.shortcutLabel {
  font: var(--l3-text-label-12);
}

/* ---- Smart Invest strategy card: a Section (12 padding, 12 gap); the head is a horizontal Stack ---- */

/* The head's 12 below and its hairline: Section pads both sides alike and has no one-side border. */
.strategyHead {
  padding-bottom: var(--l3-spacing-12);
  border-bottom: 1px solid var(--l3-border-light);
}

.strategyName {
  flex: 1;
  min-width: 0;
  font: var(--l3-text-heading-16);
}

.strategyDeposit {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--l3-spacing-02);
}

/* Three stats: left, centre and right-aligned in auto · 1fr · auto columns (Grid has equal columns only). */
.stats {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: var(--l3-spacing-16);
}

.stat {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-04);
}

.stat:nth-child(2) {
  align-items: center;
}

.stat:last-child {
  align-items: flex-end;
}

.statValue {
  font: var(--l3-text-label-16);
  white-space: nowrap;
}

/* Last week: one small ring per trading day, profit or loss. */
.days {
  display: flex;
  gap: var(--l3-spacing-04);
}

.day {
  display: grid;
  place-items: center;
  width: var(--l3-size-20);
  height: var(--l3-size-20);
  border-radius: var(--l3-radius-full);
  font: var(--l3-text-label-10);
}

.day[data-result='profit'] {
  background: var(--l3-surface-accent-success-light);
  color: var(--l3-content-accent-success-default);
  box-shadow: inset 0 0 0 1px var(--l3-border-accent-success-default);
}

.day[data-result='loss'] {
  background: var(--l3-surface-accent-error-light);
  color: var(--l3-content-accent-error-default);
  box-shadow: inset 0 0 0 1px var(--l3-border-accent-error-default);
}

/* The footer's charges + Invest now: a horizontal Stack that fills the footer. */
.charges {
  flex: 1;
}

.chargesText {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-02);
}

.price {
  display: flex;
  align-items: baseline;
  gap: var(--l3-spacing-04);
  font: var(--l3-text-label-16);
}

.was {
  font: var(--l3-text-label-12);
  color: var(--l3-content-tertiary);
  text-decoration: line-through;
}

/* ---- Highlights ---- */
.highlightCard {
  display: flex;
  flex-direction: column;
  gap: var(--l3-spacing-08);
}

.highlightTitle {
  font: var(--l3-text-heading-20);
}

.highlightValue {
  font: var(--l3-text-heading-24);
  color: var(--l3-content-accent-success-default);
}

/* ---- Invest in ideas: a basket's logos, 3 across (live app; L3 Avatar tops out at 48 — live tiles are ~80) ---- */
.logos {
  display: grid;
  grid-template-columns: repeat(3, max-content);
  gap: var(--l3-spacing-12);
}

.basketName {
  font: var(--l3-text-heading-18);
}

/* ---- Disclosure ---- */
.disclosureText {
  display: -webkit-box;
  margin: 0;
  overflow: hidden;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 4; /* live app: four lines, then … Read More */
  font: var(--l3-text-description-12);
  color: var(--l3-content-tertiary);
}

.disclosureText[data-open] {
  -webkit-line-clamp: unset;
}

/* "Read More": a text link (live app: blue, underlined), not a button. */
.readMore {
  padding: 0;
  border: 0;
  background: none;
  color: var(--l3-content-accent-discover-default);
  font: var(--l3-text-label-12);
  text-decoration: underline;
  cursor: pointer;
}

/* ---- Announcements: the campaign's artwork, full width, radius 12 ---- */
.announcementImage {
  display: block;
  width: 100%;
  height: auto;
  border-radius: var(--l3-radius-12);
}
```

### `src/flows/stocks/components/Ideas.tsx`

```tsx
import { useState } from 'react'
import { Avatar } from '@l3/components/Avatar'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { Section } from '@l3/components/Section'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { msChevronRight } from '@l3/icons/material'
import { Rail } from '../../../shared/ui/Rail'
import { disclosure, ideaBaskets } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** Invest in ideas: curated baskets — their stocks' logos, the name and the count. */
export function IdeaBaskets({ onOpen }: { onOpen?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('baskets.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('baskets.title')} description={t('baskets.description')} /></Inset>
      <Rail label={t('baskets.railLabel')} visible={1.15}>
        {ideaBaskets.map((b) => (
          <Card key={b.id} onClick={() => onOpen?.(b.id)}>
            <span className={styles.cardStack}>
              <span className={styles.logos} aria-hidden="true">
                {b.stocks.slice(0, 6).map((s) => <Avatar key={s.symbol} initials={s.initials} src={s.logoUrl} size={48} shape="rounded" />)}
              </span>
              <span className={styles.basketName}>{b.name}</span>
              <span className={styles.cardRow}>
                <span className={styles.cardMeta}>{t('baskets.count', { count: b.stocks.length })}</span>
                <span className={styles.chevron}><Icon icon={msChevronRight} /></span>
              </span>
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** The broker's regulatory line: three lines, then "Read More" (live app) — it opens the full text in place. */
export function Disclosure() {
  const [open, setOpen] = useState(false)
  return (
    <Section paddingInline="16" gap="04" align="start">
      <p className={styles.disclosureText} data-open={open || undefined}>{disclosure}</p>
      <button type="button" className={styles.readMore} aria-expanded={open} onClick={() => setOpen((o) => !o)}>
        {t(open ? 'disclosure.readLess' : 'disclosure.readMore')}
      </button>
    </Section>
  )
}
```

### `src/flows/stocks/components/Invest.tsx`

```tsx
import { useEffect, useState } from 'react'
import { Avatar } from '@l3/components/Avatar'
import { BrandLogo } from '@l3/components/BrandLogo'
import { Button } from '@l3/components/Button'
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { Inset } from '@l3/components/Inset'
import { PriceChange } from '@l3/components/PriceChange'
import { Section } from '@l3/components/Section'
import { SectionHeader } from '@l3/components/SectionHeader'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msAutorenew, msChevronRight, msVerified } from '@l3/icons/material'
import { usePreview } from '../../../shared/prototype'
import { inr } from '../../../shared/data/format'
import { Rail } from '../../../shared/ui/Rail'
import { announcement, highlights, sectors, strategies } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

const TAG_SWAP_MS = 3000

/** Smart Invest's tag: "NSE/BSE Approved" and "Auto Buy & Sell" take turns, every 3 s (live app). */
function SmartInvestTag() {
  const { interactive } = usePreview()
  const [auto, setAuto] = useState(false)
  useEffect(() => {
    if (!interactive) return
    const timer = window.setInterval(() => setAuto((a) => !a), TAG_SWAP_MS)
    return () => window.clearInterval(timer)
  }, [interactive])
  return (
    <Tag color="success" iconLeft={<Icon icon={auto ? msAutorenew : msVerified} size={16} />} aria-live="polite">
      {t(auto ? 'smartInvest.tagAuto' : 'smartInvest.tag')}
    </Tag>
  )
}

/** Smart Invest: managed strategies that buy and sell for you. Sits on the brand tint (screens/Home: tinted). */
export function SmartInvest({ onInvest, onOpen }: { onInvest?: (id: string) => void; onOpen?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('smartInvest.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('smartInvest.title')} tag={<SmartInvestTag />} /></Inset>
      <Rail label={t('smartInvest.railLabel')} visible={1.08}>
        {strategies.map((s) => (
          <Card
            key={s.id}
            padding="none"
            onClick={() => onOpen?.(s.id)}
            footerFilled
            footer={
              <Stack direction="horizontal" align="center" justify="between" gap="12" className={styles.charges}>
                <span className={styles.chargesText}>
                  <span className={styles.cardMeta}>{t('smartInvest.monthlyCharges')}</span>
                  <span className={styles.price}>{inr(s.monthlyCharge, 0)} <span className={styles.was}>{inr(s.monthlyChargeWas, 0)}</span></span>
                </span>
                <Button variant="tertiary" onClick={() => onInvest?.(s.id)}>{t('smartInvest.investNow')}</Button>
              </Stack>
            }
          >
            <Section padding="12" gap="12">
              <Stack direction="horizontal" align="center" gap="12" className={styles.strategyHead}>
                <Avatar icon={<BrandLogo brand="lemonn" variant="icon" size={24} decorative />} size={40} />
                <span className={styles.strategyName}>{s.name}</span>
                <span className={styles.strategyDeposit}>
                  <span className={styles.statValue}>{inr(s.minDeposit, 0)}</span>
                  <span className={styles.cardMeta}>{t('smartInvest.minDeposit')}</span>
                </span>
              </Stack>
              <div className={styles.stats}>
                <span className={styles.stat}>
                  <span className={styles.cardMeta}>{t('smartInvest.lastWeek')}</span>
                  <span className={styles.days} aria-label={t('smartInvest.lastWeekLabel', { profit: s.lastWeek.filter((d) => d.result === 'profit').length, total: s.lastWeek.length })}>
                    {s.lastWeek.map((d) => <span key={d.day} className={styles.day} data-result={d.result} aria-hidden="true">{d.day}</span>)}
                  </span>
                </span>
                <span className={styles.stat}>
                  <span className={styles.cardMeta}>{t('smartInvest.aum')}</span>
                  <span className={styles.statValue}>{s.aum}</span>
                </span>
                <span className={styles.stat}>
                  <span className={styles.cardMeta}>{t('smartInvest.slotsLeft')}</span>
                  <span className={styles.statValue}>{t('smartInvest.slots', { left: s.slotsLeft, total: s.slots })}</span>
                </span>
              </div>
            </Section>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** One announcement — the campaign's own artwork (live app: the Jio Platforms IPO banner); the whole banner opens it. */
export function Announcement({ onOpen }: { onOpen?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('announcements.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('announcements.title')} /></Inset>
      <Inset paddingInline="16" paddingBlock="00">
        <Card variant="flat" padding="none" onClick={onOpen} aria-label={announcement.alt}>
          <img src={announcement.image} alt="" className={styles.announcementImage} />
        </Card>
      </Inset>
    </Stack>
  )
}

/** Sectors: how many stocks each has and how it moved today. */
export function Sectors({ onOpen, onViewAll }: { onOpen?: (id: string) => void; onViewAll?: () => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('sectors.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('sectors.title')} action={{ type: 'view-all', onClick: () => onViewAll?.() }} /></Inset>
      <Rail label={t('sectors.railLabel')} visible={1.75}>
        {sectors.map((s) => (
          <Card key={s.id} onClick={() => onOpen?.(s.id)}>
            <span className={styles.cardStack}>
              <span className={styles.cardRow}>
                <span className={styles.cardTitle}>{s.name}</span>
                <span className={styles.chevron}><Icon icon={msChevronRight} size={16} /></span>
              </span>
              <span className={styles.cardRow}>
                <span className={styles.cardMeta}>{t('sectors.count', { count: s.count })}</span>
                <PriceChange value={s.changePct} size="sm" arrow />
              </span>
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}

/** Highlights: product spotlights — mutual funds (tax saving) and F&O (the index today). */
export function Highlights({ onExplore }: { onExplore?: (id: string) => void }) {
  return (
    <Stack as="section" gap="16" aria-label={t('highlights.title')}>
      <Inset paddingInline="16" paddingBlock="00"><SectionHeader title={t('highlights.title')} /></Inset>
      <Rail label={t('highlights.railLabel')} visible={1.3}>
        {highlights.map((h) => (
          <Card key={h.id} onClick={() => onExplore?.(h.id)} footer={<Button variant="tertiary" onClick={() => onExplore?.(h.id)}>{h.cta}</Button>}>
            <span className={styles.highlightCard}>
              <span className={styles.highlightTitle}>{h.title}</span>
              <span className={styles.cardMeta}>{h.description}</span>
              {h.id === 'mutual-funds' ? <span className={styles.highlightValue}>{h.value}</span> : <PriceChange value={h.changePct} size="lg" arrow />}
              <span className={styles.cardMeta}>{h.caption}</span>
            </span>
          </Card>
        ))}
      </Rail>
    </Stack>
  )
}
```

### `src/flows/stocks/components/Overview.tsx`

```tsx
import { Card } from '@l3/components/Card'
import { Icon } from '@l3/components/Icon'
import { PriceChange } from '@l3/components/PriceChange'
import { Stack } from '@l3/components/Stack'
import { Tag } from '@l3/components/Tag'
import { msArrowForward, msChevronRight } from '@l3/icons/material'
import { indices } from '../../../shared/data/market'
import { holdings } from '../../../shared/data/portfolio'
import { inr } from '../../../shared/data/format'
import { Rail } from '../../../shared/ui/Rail'
import { products } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** NIFTY 50 · SENSEX · … — today's index moves; each opens its chart. Scrolls sideways. */
export function IndexStrip({ onIndex }: { onIndex?: (id: string) => void }) {
  return (
    <Stack direction="horizontal" align="center" gap="16" className={styles.indexStrip} role="region" aria-label={t('indices.label')}>
      {indices.map((index, i) => (
        <Stack key={index.id} direction="horizontal" align="center" gap="08" className={styles.indexItem}>
          {i > 0 && <span className={styles.divider} aria-hidden="true" />}
          <Card variant="flat" padding="none" onClick={() => onIndex?.(index.id)}>
            <span className={styles.index}>
              <span className={styles.indexName}>{index.name}</span>
              <span className={styles.indexValue}>{index.value.toLocaleString('en-IN', { minimumFractionDigits: 2 })}</span>
              <PriceChange value={index.changePct} size="md" />
              <span className={styles.chevron}><Icon icon={msChevronRight} size={16} /></span>
            </span>
          </Card>
        </Stack>
      ))}
    </Stack>
  )
}

/** Current value of the holdings and the overall return — the page's headline number. The block opens Portfolio. */
export function HoldingsSummary({ onOpen }: { onOpen?: () => void }) {
  return (
    <Card variant="flat" padding="none" onClick={onOpen}>
      <span className={styles.holdings}>
        <span className={styles.holdingsText}>
          <span className={styles.holdingsLabel}>{t('holdings.label')}</span>
          <span className={styles.holdingsValue}>
            {inr(holdings.current)}
            <PriceChange value={holdings.returnPct} size="lg" arrow />
          </span>
        </span>
        <span className={styles.roundArrow} aria-hidden="true"><Icon icon={msArrowForward} /></span>
      </span>
    </Card>
  )
}

/** Mutual funds · Equity F&O · Commodities — each product's one-line promise. */
export function ProductCards({ onProduct }: { onProduct?: (id: string) => void }) {
  return (
    <Rail label={t('products.label')} visible={2.15} className={styles.onEdgeRoom}>
      {products.map((p) => (
        <Card key={p.id} onClick={() => onProduct?.(p.id)}>
          <span className={styles.cardStack}>
            {p.isNew && <span className={styles.onEdge}><Tag color="success" size="sm">{t('products.new')}</Tag></span>}
            <span className={styles.cardRow}>
              <span className={styles.productTitle}>{p.name}</span>
              <span className={styles.chevron}><Icon icon={msChevronRight} size={16} /></span>
            </span>
            <span className={styles.cardMeta}>
              <span className={styles.highlight}>{p.highlight}</span> {p.detail}
            </span>
          </span>
        </Card>
      ))}
    </Rail>
  )
}
```

### `src/flows/stocks/copy.json`

```json
{
  "search.placeholder": "Search stocks",
  "search.label": "Search stocks",
  "top.wallet": "Wallet",
  "top.profile": "Profile and settings",
  "indices.label": "Market indices",
  "holdings.label": "Current holdings",
  "products.label": "Products",
  "products.new": "New",
  "shortcuts.title": "Do More With Lemonn",
  "shortcuts.watchlistBadge": "{count} in your watchlist",
  "movers.title": "Top Movers",
  "movers.tabsLabel": "Movers",
  "movers.gainers": "Top Gainers",
  "movers.losers": "Top Losers",
  "movers.under100": "Under ₹100",
  "watchlists.title": "Watchlist",
  "watchlists.new": "Add Watchlist",
  "watchlists.railLabel": "Your watchlists",
  "watchlists.one": "{count} stock",
  "watchlists.many": "{count} stocks",
  "smartInvest.title": "Smart Invest",
  "smartInvest.tag": "NSE/BSE Approved",
  "smartInvest.railLabel": "Strategies",
  "smartInvest.monthlyCharges": "Monthly charges",
  "smartInvest.investNow": "Invest Now",
  "smartInvest.minDeposit": "Min. Deposit",
  "smartInvest.lastWeek": "Last week",
  "smartInvest.lastWeekLabel": "Last week: {profit} profit days of {total}",
  "smartInvest.aum": "AUM",
  "smartInvest.slotsLeft": "Slots left",
  "smartInvest.slots": "{left}/{total}",
  "announcements.title": "Announcements",
  "announcements.new": "New",
  "sectors.title": "Invest in Sectors",
  "sectors.railLabel": "Sectors",
  "sectors.count": "{count} Stocks",
  "highlights.title": "Highlights",
  "highlights.railLabel": "Highlights",
  "baskets.title": "Invest in Ideas",
  "baskets.description": "Curated industry-based stock offerings",
  "baskets.railLabel": "Idea baskets",
  "baskets.count": "{count} Stocks",
  "smartInvest.tagAuto": "Auto Buy & Sell",
  "disclosure.readMore": "Read More",
  "disclosure.readLess": "Read Less"
}
```

### `src/flows/stocks/copy.ts`

```ts
import { makeCopy } from '../../shared/copy'
import copy from './copy.json'

/** Every user-facing string of Stocks is in copy.json: t('movers.title'), t('watchlists.count', { count }). */
export const t = makeCopy('stocks', copy)
```

### `src/flows/stocks/data.ts`

```ts
import { msAddChart, msCampaign, msEditNote, msRedeem } from '@l3/icons/material'
import jioIpoBanner from './assets/announcement-jio-ipo.jpg'
import { stocks, type Stock } from '../../shared/data/market'

/** Stocks home — what the API sends for each section, in page order. Sample values; market data from the live app. */

/** Product shortcuts under the holdings: one line on what's special about each. */
export type Product = { id: 'mutual-funds' | 'fno' | 'commodities'; name: string; highlight: string; detail: string; isNew?: boolean }
export const products: Product[] = [
  { id: 'mutual-funds', name: 'Mutual funds', highlight: '240+', detail: 'funds' },
  { id: 'fno', name: 'Equity F&O', highlight: '0', detail: 'slippage' },
  { id: 'commodities', name: 'Commodities', highlight: '0', detail: 'brokerage', isNew: true },
]

/** "Do more with Lemonn": feature shortcuts, each with an optional count or "New" flag. */
export type Shortcut = { id: string; label: string; icon: string; badge?: { count?: number; text?: string } }
export const shortcuts: Shortcut[] = [
  { id: 'trade-ideas', label: 'Trade Ideas', icon: msAddChart },
  { id: 'ipo', label: 'IPO', icon: msCampaign, badge: { text: '5 New' } },
  { id: 'watchlist', label: 'Watchlist', icon: msEditNote, badge: { count: 3 } },
  { id: 'referral', label: 'Referral', icon: msRedeem, badge: { text: 'New' } },
]

/** Top movers: three lists, picked with the chips. */
export type MoverList = 'gainers' | 'losers' | 'under-100'
export const movers: Record<MoverList, Stock[]> = {
  gainers: [stocks.TTML, stocks.SAGILITY, stocks.IGIL, stocks.ASAHIINDIA, stocks.CRISIL, stocks.JUBLINGREA],
  losers: [stocks.IEX, stocks.ZOMATO, stocks.PAYTM, stocks.DELHIVERY, stocks.NYKAA, stocks.POLICYBZR],
  'under-100': [stocks.IDEA, stocks.YESBANK, stocks.SOUTHBANK, stocks.SUZLON, stocks.NHPC, stocks.IRFC],
}

/** Smart Invest: managed strategies (auto buy & sell). Last week = one result per trading day. */
export type Strategy = {
  id: string
  name: string
  minDeposit: number
  lastWeek: { day: 'M' | 'T' | 'W' | 'Th' | 'F'; result: 'profit' | 'loss' }[]
  aum: string
  slotsLeft: number
  slots: number
  monthlyCharge: number
  monthlyChargeWas: number
}
export const strategies: Strategy[] = [
  {
    id: 'credit-spread-alpha', name: 'Credit Spread Alpha', minDeposit: 80000, aum: '92L', slotsLeft: 356, slots: 500, monthlyCharge: 0, monthlyChargeWas: 300,
    // the live app lists the last four trading days, oldest first
    lastWeek: [{ day: 'Th', result: 'loss' }, { day: 'F', result: 'profit' }, { day: 'M', result: 'profit' }, { day: 'W', result: 'loss' }],
  },
  {
    id: 'momentum-swing', name: 'Momentum Swing', minDeposit: 50000, aum: '₹86.4 L', slotsLeft: 112, slots: 300, monthlyCharge: 0, monthlyChargeWas: 250,
    lastWeek: [{ day: 'M', result: 'profit' }, { day: 'T', result: 'profit' }, { day: 'W', result: 'loss' }, { day: 'Th', result: 'profit' }, { day: 'F', result: 'profit' }],
  },
]

/** One announcement: a campaign's artwork (from the API) and where it leads. */
export const announcement = {
  image: jioIpoBanner,
  alt: 'Jio Platforms IPO is now live — bidding open till 23rd Oct. Apply now',
}

/** Sectors: how many stocks and how the sector moved today. */
export type Sector = { id: string; name: string; count: number; changePct: number }
export const sectors: Sector[] = [
  { id: 'building-materials', name: 'Building Materials', count: 18, changePct: -0.05 },
  { id: 'consumer-discretionary', name: 'Consumer Discretionary', count: 22, changePct: -0.25 },
  { id: 'financials', name: 'Financials', count: 41, changePct: 0.31 },
]

/** Highlights: product spotlights with one number each. */
export type Highlight =
  | { id: 'mutual-funds'; title: string; description: string; value: string; caption: string; cta: string }
  | { id: 'fno'; title: string; description: string; changePct: number; caption: string; cta: string }
export const highlights: Highlight[] = [
  { id: 'mutual-funds', title: 'Mutual Funds', description: 'Invest in Mutual Funds and save taxes upto', value: 'Rs 46,800', caption: 'Every year', cta: 'Explore' },
  { id: 'fno', title: 'F&O', description: 'Trade in Futures and Options', changePct: -0.54, caption: 'Nifty 50', cta: 'Explore' },
]

/** Invest in ideas: curated baskets of stocks. */
export type IdeaBasket = { id: string; name: string; stocks: Stock[] }
export const ideaBaskets: IdeaBasket[] = [
  { id: 'oil-price-dependents', name: 'Oil Price Dependents', stocks: [stocks.JINDRILL, stocks.HINDOILEXP, stocks.OIL, stocks.ONGC, stocks.RELIANCE] },
  // the second basket only peeks in at the screen edge on the live app; its full name isn't visible there
  { id: 'high-growth', name: 'High Growth Media', stocks: [stocks.TVTODAY, stocks.NTPC, stocks.IRFC, stocks.COALINDIA, stocks.HAL, stocks.SBIN, stocks.NHPC] },
]

/** The broker's regulatory line (shared with Profile). */
export { disclosure } from '../../shared/data/broker'
```

### `src/flows/stocks/index.tsx`

```tsx
import type { Flow } from '../types'
import { goToTab } from '../../shared/ui/MainNav'
import { Home } from './screens/Home'

/**
 * Stocks — the Stocks tab, the app's home after login. Built from the live app's screenshots (shared 2026-10-17,
 * re-matched to the live app over ADB on 2026-10-22; there's no Figma for it yet). One long page; the board shows it as frames scrolled to each part.
 */
export const stocks: Flow = {
  id: 'stocks',
  title: 'Stocks',
  description: 'The Stocks tab — the home after login: indices, holdings, products, movers, watchlists, ideas.',
  figmaFile: '',
  screens: [
    { id: 'stocks-01', name: 'Home', figma: '', note: 'Top of the page: search, indices, holdings, products, shortcuts.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} /> },
    { id: 'stocks-02', name: 'Home · top movers & watchlists', figma: '', note: 'Same page, scrolled.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} scrollTo="movers" /> },
    { id: 'stocks-03', name: 'Home · top losers', figma: '', note: 'The "Top losers" chip picked.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} scrollTo="movers" movers="losers" /> },
    { id: 'stocks-04', name: 'Home · Smart Invest', figma: '', note: 'Same page, scrolled: managed strategies, then the announcement.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} scrollTo="invest" /> },
    { id: 'stocks-05', name: 'Home · sectors & highlights', figma: '', note: 'Same page, scrolled.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} scrollTo="sectors" /> },
    { id: 'stocks-06', name: 'Home · highlights & idea baskets', figma: '', note: 'Same page, scrolled. (The live app no longer has Trade ideas here — removed 2026-10-22.)', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} scrollTo="highlights" /> },
    { id: 'stocks-07', name: 'Home · idea baskets & disclosure', figma: '', note: 'The end of the page: the disclosure, four lines and Read More.', tab: true, render: (go) => <Home onTab={(t) => goToTab(go, t)} onSearch={() => go('search-01')} onProfile={() => go('profile-01')} scrollTo="more" /> },
  ],
}
```

### `src/flows/stocks/screens/Home.module.css`

```css
/*
 * Stocks home — page layout. A grey summary area (indices, holdings, products) and, over its bottom edge, the feed on
 * a rounded sheet (radius 32 at the top), like the live app. The grey area is intentional and high-emphasis (the
 * account summary), so a large surface/secondary block is allowed here (DESIGN_SYSTEM 7.2). The layout is L3 Stack /
 * Section; what those blocks don't cover lives here: uneven padding (16 / 48, 32 / 24), the feed's overlap and
 * top-only radius, the default surface and the accent tints.
 */
.summary {
  padding: var(--l3-spacing-16) 0 var(--l3-spacing-48);
  background: var(--l3-surface-secondary);
}

.feed {
  position: relative;
  margin-top: calc(-1 * var(--l3-spacing-24));
  padding: var(--l3-spacing-32) 0 var(--l3-spacing-24);
  border-radius: var(--l3-radius-32) var(--l3-radius-32) 0 0;
  background: var(--l3-surface-default);
}

/* Smart Invest sits on the brand tint, edge to edge. */
.tinted[data-tint='brand'] {
  background: var(--l3-surface-accent-brand-light);
}

/* Top bar, live app: profile initials on the left, then the search field and the wallet (the L3 Actionbar). */
.topBar {
  background: var(--l3-surface-default);
}

.topBar > .searchEntry > * {
  flex: 1;
  min-width: 0;
}

.profile {
  flex: none;
  margin-inline-start: var(--l3-spacing-16); /* the Actionbar brings the 16 after it */
  padding: 0;
  border: 0;
  border-radius: var(--l3-radius-full);
  background: none;
  cursor: pointer;
}

/* Wraps the top bar only to catch a tap on its search field (opens Search); adds no box of its own. */
.searchEntry {
  display: contents;
}
```

### `src/flows/stocks/screens/Home.tsx`

```tsx
import { useLayoutEffect, useRef, useState } from 'react'
import { Actionbar, ActionbarAction } from '@l3/components/Actionbar'
import { Section } from '@l3/components/Section'
import { Stack } from '@l3/components/Stack'
import { Avatar } from '@l3/components/Avatar'
import { msAccountBalanceWallet } from '@l3/icons/material'
import { user } from '../../../shared/data/user'
import { MainNav, type Section as NavSection } from '../../../shared/ui/MainNav'
import { Screen } from '../../../shared/ui/Screen'
import { Disclosure, IdeaBaskets } from '../components/Ideas'
import { Announcement, Highlights, Sectors, SmartInvest } from '../components/Invest'
import { HoldingsSummary, IndexStrip, ProductCards } from '../components/Overview'
import { Shortcuts, TopMovers, Watchlists } from '../components/Discover'
import type { MoverList } from '../data'
import { t } from '../copy'
import styles from './Home.module.css'

/** Where the page is scrolled to — the board shows the long page as several frames, like the live app's screenshots. */
export type HomePart = 'top' | 'movers' | 'invest' | 'sectors' | 'highlights' | 'more'

type Props = {
  scrollTo?: HomePart
  /** Which Top movers list is picked (board state). */
  movers?: MoverList
  /** The main navbar: go to another tab. */
  onTab?: (section: NavSection) => void
  /** Tapping the search field opens the Search screen (search-01), where the typing happens. */
  onSearch?: () => void
  /** The profile icon opens Profile & settings (profile-01). */
  onProfile?: () => void
}

/**
 * stocks-01 … 07 · Stocks home — the app's first L1 screen (the Stocks tab).
 * Top bar: profile initials, search, wallet (sticky) — as in the live app. The search field is the way into Search: tapping it opens the Search
 * screen (stocks tab, keyboard up) — nothing is typed here. Then, on a grey summary area: today's indices, the holdings and the
 * products; and on the rounded sheet over it, the feed: shortcuts, top movers, watchlists, Smart Invest (brand tint),
 * an announcement (the campaign's artwork), sectors, highlights, idea baskets and the broker's disclosure.
 */
export function Home({ scrollTo = 'top', movers, onTab, onSearch, onProfile }: Props) {
  const [query, setQuery] = useState('')
  const page = useRef<HTMLDivElement>(null)

  // Board frames: open the page scrolled to a part (the screen's body is this page's scroll container).
  useLayoutEffect(() => {
    const root = page.current
    const body = root?.parentElement
    const target = root?.querySelector(`[data-home="${scrollTo}"]`)
    if (!body || !target || scrollTo === 'top') return
    body.scrollTop += target.getBoundingClientRect().top - body.getBoundingClientRect().top
  }, [scrollTo])

  return (
    <Screen
      top={
        // Live app: the profile initials on the left, then the search field and the wallet (an L3 Actionbar).
        // Focus on the field (a tap) opens Search; the wrapper only catches that focus.
        <Stack direction="horizontal" align="center" className={styles.topBar}>
          <button type="button" className={styles.profile} aria-label={t('top.profile')} onClick={onProfile}>
            <Avatar initials={initials(user.name)} size={40} />
          </button>
          <div className={styles.searchEntry} onFocus={(e) => { if ((e.target as HTMLElement).matches('input[type="search"]')) onSearch?.() }}>
            <Actionbar
              search={{ value: query, onChange: setQuery, placeholder: t('search.placeholder'), label: t('search.label') }}
              actions={<ActionbarAction icon={msAccountBalanceWallet} label={t('top.wallet')} onClick={() => {}} />}
            />
          </div>
        </Stack>
      }
      bottom={<MainNav value="stocks" onChange={onTab} />}
    >
      <Stack ref={page}>
        <Stack gap="24" className={styles.summary} data-home="top">
          <IndexStrip />
          <HoldingsSummary />
          <ProductCards />
        </Stack>
        <Stack gap="32" className={styles.feed}>
          <Shortcuts />
          <div data-home="movers"><TopMovers initial={movers} /></div>
          <Watchlists />
          <Section paddingBlock="24" className={styles.tinted} data-tint="brand" data-home="invest"><SmartInvest /></Section>
          <Announcement />
          <div data-home="sectors"><Sectors /></div>
          <div data-home="highlights"><Highlights /></div>
          <div data-home="more"><IdeaBaskets /></div>
          <Disclosure />
        </Stack>
      </Stack>
    </Screen>
  )
}

/** "Rohan Mehta" → "RM": the profile's initials. */
function initials(name: string) {
  return name.split(' ').map((part) => part.charAt(0)).join('').slice(0, 2).toUpperCase()
}
```

## Source — shared app parts it uses

### `src/shared/assets/logos/asahiindia.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>150</exif:PixelXDimension>
         <exif:PixelYDimension>150</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
��Q�  -�IDATx�}xTU��;��7�t��b{Aݵ7�UW]q-*ؕDqſ��Ȣ>�Ů����K(!!$��L����]L���	���t��̽�{���rT999a���]��❅�a�+"
)��w��J��K�V��K��_��	���^�GFF�v튼�<dee!99�&�IA� �����t*���
����������w�p�V�F� �HD���%F[�����c�9#F�@������� R�*/��A����\.�v;�n7l6��ك�k�b�ƍ(((@ 8tz��}q�~P	2X(T� �$2g�}6F��P�Pfm��z��ҭV+֯_�O?�6l@YYjjj�yE�<V׌�8�5��b�^/RRR�0` �=�\�3���\�V�����b�
,[�LA��ݻ����r�V�K�
b��^x�8��Wjjj�o���	����o�����Y��h�6�6��Vy�֭n���q��ӧ�a�X�!L�P��o���n��:R#�p�"�|5��իn��F�j;u���z������W_��_ĦM��\��#ݡ�F�l�~�p�5���Cff�a���NL���ŋ���BX��o4M�����C���Ν;�n��W_���:�5�>����������ؾ}{B��p�%��s���	0r��#։�ڧL��`�|����%�9ސ0��v,N�[o�7�|� �F�S��_��s΄�ޮD�p.�Ϗ<�v��w/���G�}�B��`qJ̚5W^y�bJ�������i�� �7�VQ���Ac2F;4.߉S�o߾8餓P^^�x����5��2y���)S�����8+:
�t?�%�h��� |[v�婁����R�O$Ѓ���;O�h�7oV����`�\q+N�6��v�����u�寶��J�j�bB*�*��ax�Q�yZF���L"AL'��jժ� 9n���SO=�h�:
|"�>=X�i�8L�p��	CUTA��7�ca`b�0����_���=�Xlٲ���W��X��1G��Z��JtgΜ9�7*V���0-�;D'JՒ�}���.�����3�1u2�����|${���؅@��0��Ay�x7c�.]p�'b׮]߶�Y,�-��1�)�ॗ^����[����V����>�ҫ*�Ӫ��/ȹg"R��apU�`X�zj�����-���J'ϙg���.��We]ڋ�"Xd�h����S܎q�\���b�^x���A��;�ȝ��N-�-}�U��"\\���-��u�K��-�Jl�صW��w�ء�$��"s�;�8̟?_Qb{ۭ��7_�1cR�� ���F"��{a�߯�`���{t�w�f��*���ñ����p��bu,,,Ķm��Ůc�`���Ν;�59t�E����zr.,�vB�S�>h 2g<�$*Q\�C�F>�tF�[�7n���*[5l���&ss"�%�](YF�_���m����B�=z�P"'�e��`_��OF֮}��C�dL��̇�r��Q��̗���Z��Ȭ��'$;k`�]��-�a:�x+��안�%�-vr�,2W&����H�\I��(�3�6A����-)�NB��-���j ���M0�TC]i���C���evLI$��믿n���f��,OԤI�p�M7�D�o��ܸ	�OA�r-�����Ӕ��}�X�Zh��5jX�>j�������*�����'@g2�g�m>Wb�*���n���fk�袋��C%4�m�|4��|�/V�P�i1Cw�mȼ��[邔��ث7�U��w�F��};��IÇB�`oW�~�d|��+�����OV�T�du$���p`�,X>���՝,�M�Y��V"7r�D��c�滇)�).?[�����D�M���DdN�.�.q�H�HK=���`qd�y�̙>|x��'����es�C��ǰ��P��0L���\��}~o�p}��a��s�o����������6�~kY\��M}@����-R�Z�`Q�$c���b��~��@�[K~�5�\Ԑ���^��;�]Jl�/5d�������|�]5��܍pv6��qb}[͎'Q(Q�>��3%��n��z%����O+T��l�t@�vw��/��֛H��D@e���K�s�0�d���Z.�q`�Ż�ݻ�H�·9�,VO��!�����+ޮ�L�V!X���ٳq��);B� �_~���#�f�S����D�C��LLmF:L�[���iP9}���~RS����ª%i`���J�fST�b���K.�w��b�;)g�� �9�Ze�������Ⱦ2��㛴��J
��=tc�+K`�9��UD�f�;�%����N�i~���&��"kmyƌ�޽{��֢���/�EGFI	<*=B'��y
R�&ƃf��鷶m�3mc������ˁI�%�2Y��|F�!K�iQZ�$�_u�UJ�W����l,��Κs�~��A���G�������2���q�D�3,�]ЭZ��E� �7~R�~�FI^l�Ep$3cܸqJ�^c��{:��q���`��Y�Lg�Mb�+�(zs��g�8m:��]���<�0��j/��2�v�i��+�e�b�;�_}��>���NO�a���Ǎ�	�"��S;l>�`��A�K<�㮄���ͤ�]�Ɵ�����&��D���x��sK��e���	s�����xGC�9���S��0�ґ y\�_~�;�D���$�:��@�LZ�dA:���25�ѐ�)�ޒ(� 	+F���D�LP"E�wI)
_}��և�ȕ�?������^{;�pt{�:4�����x���Q�f&~Oy2	>jU��u�*[�Bo"/߾k��Y�}�8a/,@���@2L�l!ܶ>4�`qlH��>{J^z��!�iE��p�.H���}�C�5+W�XQ�L���^F��/L ��J,��!*������՘]{��|�����!"��(���vG�F��\�#$̘s�8Ԝ1���%�yl�?�8!� �N�P�MGE�P��V�)I<��4��䨁]����kҝ�'�/었�׾�������אA�B0��k�<�Z���U���|��`���1!2bT;X���D(��]91V�"���,�{�sH)���R#x�X��;�6�vָ���������Lh��S<�Fǉ�Ϫ"BǤ���*�l�S�{�P\ΪV@�X�`44]�c`Bӹ<��P���`s���%)=����x�h��Z�t�xA��
y˓ n��p�'&Ot��?���H�� �1�=br&N�o����P(�זo��"+o� �_�?�L����B��Z��f��VI�B���ɘ|��H6�����*���v|�SA�נs#Y�郳q���1�/��x�R�8!m��#�� �~E��3�{�I$�;�hc����={*��޽{$7�`�;��,Q�xV�� �~���姐���
�Q�"c�$���ǽ��x�k����l(q`]����=���!��_��~��o�|pU���\X�e]�l.Lu%^���ψJ��d�����ȉu[J���Eϼ4"Y����w�@R2�~��eERq9���a>q$�q�|I38ɥ��qz4x�閪@qd��r+?��C+
|a5\��Gփ�)��mA���F�ZPIo�֌j.��)<�U�<t?���I��x4F��z�XN*�>��'���&����drzL�6�QC���0����J퇆��A��#���������1~�5J{��=�Ni�wB��JѦ X�.dOp��q��&���Dg��kdN�3�#�%U�D�RP2/��/�����3,��F�),V$�󜃇�.���QX�e?��p@��`�5P����0��G��~R�����!WH��0�x3��0�Cj?\��^��fa<@*�G-����?�j�nCգ�`޺z^�ѳ����\sͦxU⋥7�\A4��0
Kz�ƅx��J�(��yW������RU^I�cT��U5���xq�p�)}��ٲ<i�����e���I�b�[�r��
8/���X8� ���%�lI/+���s2����`o��`��ܳIp����ZX� 9Td�A�Gsر�#��Qct�B}���_���u���oe�҆e ��`ܨ<�q��y����^�k�]8�_.��:��.=�� �Y���ɢt����Ⱥ�F��)M�ުߤ�r6#]�W�2���x��r�b��}����]a��z��=7&��~���vB�F���/'�������	�&��B����~�nVD��X
�>�_;я\z���з��,;�Ҩ\����;�ޘ����댔�&��t���{0V�''�*����}����s͏����Rk,� ���k�UOσj�0�?�Ò�� �!�XU
TոP�$�R�NMѡ�I��U^x��JگuA�U�� Yle�(���� l��� �d�)ץ��}:�g�T��-����6df��w��@�?~��?�?�u�p��~9Vk/,I�r�u �ޫ m����}#V��+/B�+����X���S�],j���A�N�� �[��N�! ~~,<\@V���h	�-+ܝ�J�S%�1�x\��H�H�E����G�!x��z�`#r�U��;���a�3n�`Iǌ����wx�m�^8)CW_�N���lP��(�[=psj�N���<#90���Q�&%��S� ��O�ρ�H�ĬUQ&#l@�K�v��>}�}�dT��o؇���Ϟ��u�br=�eHzm�*k�O%����y�vTӿ����ȥ�a����=����`�AXR��Z5
����e*�IDc����j�_����uU4c�!�E*�ے����ڗ����Ð:c�G��h�Z�U�v9����!
`�A|Q,���Z�(��˯-�����y�9��wc\y�kA��TJ@Cn��L
����*:@�l�C�����IX̤Ι)��H�g������~�ޒ�ث���>�&O@5�n5��y�`���f�1�x���m���g���/#��
�V癧�˴�0��ɚ���K��R�j���T�:R���:�J&�Gb2�7�?�1V4p��٢��e;ݸy�����=�+�����KY�Tz�&���4�^X6�@��g�.�׶Ay��$�ЀEY�*�����#�`ĉ�c�"�)H�?{�ՍB�j�zt��B��,�Xh$���a���Le	��9!$Q3�7� ��������6�<�d#��:��F�����"9���`��pwI���|�+Q�`>{��|�ZG
��i��h�[��k�#�~�ܷ������?}���'"���`f�t<����.R#CF:j�wC5]�8"�D���r���.�52THǈ�XC���'w����ﴩU/�+KU�{�F<��O6�n˽2�̛n�v�$�e�`�.�_�>�=�(�{
Z=� 7*��˶���@%U}sA1�~���A�=S��~��J;�<�)-�'wJ;�B��i�0RD��������C ���+)�T.�X�~�`�.pH/*47Ǣ���hW	��sb��`wP��1H�f�E��x��U�c�*�+R�@���(�-��9�M��I/!���e�SN]��õ�r:9\!a��]5|����y�
�*�	�����q�5��l`P�N���H��J}*`D.OR��t�ބ�WU⻍���?t�@��Txi�5:�X��SAl$,��uc��,F��w�����:z
��*P��s�67�v�F���{}N)F���8�W]�\)��r����5:����ѻA�p�?��[��L����k�8Q����yPEN���"8Y+KP�[>[��;�2F�b�w�qȸ���6ݨ�#q��N\E'F���<}2k��x��W��2�� t�@�mJi	+UQffѳ��|03�o.��2TX��������K�UX��S��ـ�W�9���4nZ�b�������Ւ��QȤ�XB񢥰=>Il���9�z�td�z)lZ�\6@��c��Y�;�*`��d$��B1�W��4�)�������Wj��R��.RI��8��v���%$Ow@�
nG@�/���F$�!�\�����cl���>L�	ѕv�͖J��Ĩ=B�>;V����H�[D��0'k�R&�3���('*�e����(
9��q!��Ν�L���(�/!PS��О�~n��MԘ�!5�*��l�|�����5�)V�dS���e�P2��.z%�������O���S����,T��01`�-�g�ST���j���:s�iZi;{�B��l�%�����f�Q��ҽ��m�uDr֍ף4������pJ���P����믃�r:V�6R�
B�J&Ky��̋i���~)OH��N�n�1.,�`"g�Y�2��#֢NI��蕃{��a�5	Z��PKzOD��zr������y0��L����- y�HӾ�V�B,X��0��م�t�����A�dM>�2J�?����b�����uW��)�B���ղ��N�}��
�f���t5��rc�H�7�Y�t�H4����Ņ�t���1�T&��}�6���� <�8��.���AY�N0M���?��b�A$����&x�����(npF�г:"s�M�I�
��F����o�IJ�} �õ;
�2�󿰸���J��@\S:�b�_bg�b�b�=�E̊�Ѣ����c�t���'Dfsl��mG�.�͙�ԕ?AOn`�JFʤېu��D����qh�4ٲ'*�%�6��g)E�S�=�t�4��A�t���Q����/j3	��̢T�
Q�ʀ9tH��^ͤf��lH�|��
������$v2j"I�J�;$&�n$	5���x`�/�}Nk0j�z���P�f�>h�'"�+�z���'��7��*/� �z���9
��6+~�Y���;���$���JY0c��Wa���N�F�#Y3�KC�f�o�̇֜F�)�ʫ��A9D�0/R`���Xqj�}{Q��p�p�����}V&��؛v1y=r3(������4+-���`���0��x,�/���}5�
PPP�Ppˮ�ҵ%�0'��i��B� �5?㨝��zx&�t�vjSsm���j7vU��"�u`���q�G+�.2V�?�c��(5�HQ��=X�K*�8a���a>�Է��cG��헲��w��L�@��n,�N�,	9�z :�TEakt��~s�ʒg�C�Hzऽ���:�����N��_i�� 'K�K��d{i6kHgSn�=*T<lCp��H�W�ʆ!H{9��[}9��M=�TN䥩j�!d���ztJ1��$"u̹��4��Jh�H���*ƿWoًU%A~QĽ}����HeD�O9��'۱��J�L8��,���Ӻe�F[DS1��"v�spL7g�����z�O����A8j@�b?�V��@��A�}�Y����J
�������������i�}�d ��_�c�Iř��(kba�]�l���8Rpa5ӈ���(�K����R��b�n�(�'�abL���vV���r��fˤ�G�Fg*F-r�ǟ���B�ژ�� �����;�n�� !�Y�,	|u�h�`A����[4��r���~#<}zBǅ�n����\�	[%�S���HebA^zC���a�l�?���\�7��A%ߐ����h�D�6��I�tn�����Xǲ���XS�A5m쐎���h�ʃ�Gv��m�C�_�w+XB�<��]�>>��lVn�Sy�l�QQQ����·E��q��x��i4��_����ӥ�>�&�O+b���7�ǅWS�&S�J�a��bԡw.��`�b�#�Vz����_����l�$AUM�afT�L�k�D�*H��	f��u��d�1�zW���u�J��+�f�>�G��a�|�a$�O�K����a!�:,S�\���b��i�)��[3�O�?n�=9	p�ۀr&��[�vr��{���w�r;�L���΍���lM��<�32�Y����)�&\:����IwN�b���	�Iu�P�5��p�.IG���}��
�0'1+��^�Χ�"�	>��9t ���`+�f�m��eee��_��;���]�-Xe+ٞ.��6���q������7����`6F��g5�|x<~�򑾽���J��z���r0�_g�|��uK�5�uC�^=Yy�&�Zrq��J<���!�����i�e�ɫ2��gz�C��駝�FY<vDw\u�Q8��D˚;�Ȋ�O"s-w�v{q��ɼ��1��>�����+��#� X�Z�{�s/��b%y���l�t[4d^w�!��H�h�-z���rם���O!;�z�(���1�L��H�S�aP�,�Z���l�4J�o�!Mv��s���P������℡]`c��M
�9��t��Td��h����y)k�(��*��)�ݪ��m������.��^ѣ��L j�;A�0-�A���~���K�h\���0��~��7\Ͻ}őqd��@9J�͆��a�P�eB�}��q�EH,�J�&�(E�t���5��rA�q����6�ʹ�v)�kHa�Lñ���];5�v_'@���y�aZ���P��f-eʝJx��+��m�ćA�|�����M�K�@�d�t3g�V
�Lǜ Kn^�.���u�ʺ#��y)���.C�U�r�(�A���p���_��j��@G~��xوI|ӲgO��D��L�����z��0df$�����(��/��~�ݬ-���n��/-��x������7h�R�i�"J�9�S��6w�Aژ�a`$�H-�&s��ќN���ꫯ��O?U(Xp�FY� ���I,��`hhg�p�+�M���� Zk���'�	���QA���F�L�Y�O����N�d�������Pc�F��H��}�����|������Y6����j+W��E���d�J�r;$1��Y�O�c���p��/�%�Y��s8��Z6�)�~���x[rZ̏�,�O>��ѝI�D��F�-��>�裘O�5z�<w���A���P��3���5׮�}�Z��b��B$3͵�/��������~�K�,��'O��.rf� ��X��cF ��{ags�4:�����xl&����OU�W�kҷ�$���? ��e�8�N�h��t:!ʕ$i4�ۣI-:2��5Eђ��x�Њ\/ڻ�|0�-���w��U0�=�v�N`�w2�;ں�h�k5��{_!J�?��_�1��bRa�#�*������|DtΟ?���9�q-B��(O� N;�4�S�����dj��b�(#C�w!�Zc��Uv+RN��=���]X��O ��լ6d�Av:4�o@���(�ƺ�h�i�w��{�(�&a�5��c��2��:e��/��ɧ��b�Y�ASF���l���6��:�V;�7���%{�`��O��l�mض�m���&"����r����?��OE���9�>�����+V=lذ��$����F��Yf綯Ԧ3Y��ܼ��Xd���� [V�~�C��y_7\��[o����R<Fb�`)3jL�F��j�Z.I��{.RSc�Ood2�y�2�h<�lp뙃d���)dYظ�zC{��xq՛o¿�%��/Iץc��X���#@���?�lJ)}���V!X� �>�x���rs�����t6� c��-���`ݴδ�q��@�n��7߆�߯�Ȏ��KUXu��V�&vs�(���_V�Д�̯��e`a�+��+ߒE.�w�ʶ�aj�5[�a(�����8h�����G�6�-DV\��=�,����d*����=�n3pT,o�Uc����Ń>Ȕ^O��92p�,�k�U\���ٳgd�y��Sm�^�wKW���{�!�����<	-�����pϞ���Vv�e�婣���t�գ������l�<e�EjN��}�6!X�.�m	0�3F��T{�D~���r����֐�Z����Ll���H���Pɜ0��'��M4�֬8�t~��=z@k���^�E�I
��ɓ��N�֊�6#Xf&�Y����SB����բ�m� e����r2�\�+46�%U�OHu�A������e*|�;�C`_箝���Hb��tH�v[���[
Q�V��rA�B�;�r�D���B�Xah`uD�����&R�z�^X��C7l(�Y����B�j&7�^ä��z��fA��z�<��r��Jnܼy�R�M��Zh��b�`yI5�LB�,�.��oon���+�7��|h��K�4���D�
�f�T<� R׬�&��r����8��C���_{�5<��J�Xk�n��o7�e�ې�&	/�5�C�,�1���*&��^���O�z�U�����ʹ�B�g�3�ދ��T��*�Kd�j/X">Ku�+���3f(s[�+s�	�e��b�]�VIil?[96 �aK�~T��9���HaHh�nجհ�:��TT���9�?��*�ӓ`������_���n��� 9�D�ڃ\�~�,�	�En���Oʓw�q�*����J��u��8�m#,�n�#��?ɪ�w�@�gš���u�1�����ga��?�<z�!��՘�]+��D|�Bɢ]��M":
$�h����R;Ns��-�M(��o``}�S���ˑ�N��(2:s�s���x�g q�X W�sˠB�"G�F�����&Z����1�Ў}�P4� [�A���]hg곛�9��=H�o���x��ו�C�˖k�1.�Dd�]��z�"��Ծ��>k�V5��*Vl���DvX#���� �M���Kq�=OI�z��G��{�)_G8`�c��9n�I	�e�b��[aC��(3J��2�!�a/)���:�4�<�~֬F����wߍU�V)kk��=���Ey"�6�|۰a����Ĕ;�١��V���ƗD�'$C�dQ�$2���yD�.�ǅdL�ˢe������[uGĔC��~���Fm�̥�(з�~�9s�@�Py�#�,^k�PGnB�,O����?^aۑ��W�ŝ�럥$�·+VZrs��!�I	5�-���3��ĉ�]Ǜ����䰉v�x�b���<�񐵍�K�!82!�_�FNR�|�7*��4"�J��ʐ�t)�)`J�����Y�H�P�UW]�K.�����E�̩-&�/���F6"g�-k��a���#�[��;���q��+�`d����:%/��R'$�`�fō��a���DE-Z��*~mA��ѣ�MXb�I�1y�w+��[������
�9-Zg2�Ĳ�񩧞�.�@Q���mf2|����%���{|���J�$:��G��Gp��.�[^�����GB���}�K�����U�#�a�+/q�
�J*�@G����{?�\�f��ʅ��]ع��={*l=��I�%�P(LX�|��9r�P_�]D����B;���
B�X9_^m}h��=Q���H�����\ط���"��p�g9V^r��o�_�b�w��;b=�v�'�՘�#H��.��>Ҩ�5K� D�>�1W�    IEND�B`�
```

### `src/shared/assets/logos/crisil.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>150</exif:PixelXDimension>
         <exif:PixelYDimension>150</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
��Q�  UIDATx�]�U��;�LO$� "YĀ$QP�'*OV�5|�O�İ���'���5,뺆ր�UA�]3��9� �09vz���S]�=�i�i�;_MUW�x�{�=��S�L]�t��0>������V�1�L&��٬~��o�C������o`}��P�h���x���v���fCff&rss������,��v����l�ohh@}}=jjjPYY�^3?�Ţ�3�O�����9G"G%�a�=ꨣ0h� 2G}4�����:uB���
h6�y0}]]jkkQ^^���R���(���ʰe����OX�v����s*���`��G�q���'�N@׮]A0e�	�Z�pgv�#??ݻwo���ÁPTT�U�Vaٲeذa��Z��:�M�}��.�<x0.��B�u�Y(((P��-�K�^UU�]�v��?ƢE��m�6p��p
h��hL;��v0	�y��j����4i�L��޽{kmi7g���^{+V�����q��A5�
��=��W	,�/�0a�5�=��WWW+�?��#�����s�'��*�[�55�#�ꫯ�\��]�Z���·�~����c޼yJx�hn����c)<���]t�O��^�z�y,Yh�u��������9OS�nM��n �T�g�}6��Δ��t
dO=��/_�T2����`M���z�7��NS=�5ܞ���d�<��j��ĝl�m��t:g�%(Sݹ�{����V	S�ɲڲm-�EKռq��)�Jaa���%�@^��3-d}�`
Q�'�x"y�\z�J�m�(��sUƏ��oT�h=#0� �p�W9j�{/���������Ƥ2���W\�9s�(z�F�&��v��f�iqz衇K�u:4R�j�9眣̞T�r"��8�[`�@\x��KN}#�����ѣՂ�ʕ+��'^��`ε�֨Y�f)3cې*uK�D}�)��gϞ��oԼπ0��9��R�5j�~�i��.�۶��&(�pP��C:&�
�6rǎ�ٳgc�С���Lϕ2Z���B&��T�) p�>��c�&R�#=�1c�:٧O5��K��Lv��裏�&�x	O:D�}�6�HN
��_�~x�ᇕ�L<�I�	O��gΜ�|�(���=�cǎx���{����-S�,w�q����KH`F��n�	��~z,���@�>�^{-.����h�H`
U\Ý1cFB֗h*z�ǡ��w߭"��OqL3
Uw�uWJ������{�r�� �K��?����r8/U�0b���w�SN}�,N�0�]�_=N=��T��aSO�ǚ�p� L�0|�pL�6�U}�DZ�!\j���ەKqK �4�9r�CE7�F�p�(@�	:(&`fv��+q蚖.Y��5�\���hN��zs��ݟn�!͚5
�3}ۮ��:e�4���j�UW]�c�=�7+]��t� �'\�
`� ��I�*�E�p��%���h�#_8[uT �=sBO��䀒�\�DKSq86�"�4js�1���H�_N6 ���&�!���4�U�0+����Sb��w�yj��V�X�{�N��^L�S�^�i��\F6�"�|Ղ^����)�Wn馬��h`6����4���Gn;wP�!H��3]F�!5(��H��:i!"��t�b�H�ԡ }�.���`�#L�y:]r�!u(Э[7�|���
���'�T�##غ#���
Qi�����vM�uO��!%)T��L�&�ӯz�$�!�n0�3=�Җ�:��&{U`�����vy+kа{7ܕU0���ѽ,9��ZɼnL4��/RH�O2��mS����V@0�l�`iҷC����µ���R��iٹ#l�:�$�悾w�5�[��H��=o.�x�������֬݀���n�O�큼	�H�ւ�0�=y�F�9���"��>����'��uܙ����?
�1��6t ��Bƀ��$��+z�5x��^�[`3G��R�o�ƺ�n،�/�F�'�aٳކz�Y����s�D�����5�N��۷��6ΒJԚ�A��+�3t����j�x�d�����K�q����ָ���n�z]n�
��{	�͛଩��-�׃A��
Q��h�u::L8C��i�����O��~+�f+$-cf����� ��EY(����'��U��ڹS��>�J����އ���L;  �W����?B��saߴeb���}����c�Z�[W��ϖ����x�M���[��e�.^���l�=Y�D:���}���9��R��#C�H��& sA�#8����Q<�-��<����A.T[eԞxl�;�n�ԭ݄�R8֔�|�ch��Bש�����f�������_~GD���TWKY%2k�`�W��M;P,#�������R� @6�V��--׬�1��U�3��p���G!��Ap���S��f`�d�J[�Y���*t�dʛZP�b���,���C����z���I��]�C�$�G�dQ\[L{�X�\s^G��b4�,��{�?��G�s�<d�V�f�(�Elߍ�m[P9{.�ǜ�y���ӫ��1jB��b=2�^ @m[��>}e{�P9o�~؀L�z��>��l|_J�HaN�|��\�%�y�-8���jG݈�ȟ1ك��%�]XE�ν�\�5����*a�_�D�[�y���g�V*s��S�s���
��h�h�s�,������'TYk�����i�҈r1w��u
&��9 �8� N!�T�߃�;Pߏ�Q��7ȸ�09�"����y`r<r�7��U�'#�6�?���a�� {I1j�}��1#a���mf�ݫ�+�y�Z�L6�w,p�E�����l;����¾�{���1r�eU����˧�ޥC0n�L�/3��Ǎ�j���p]1QGͮ"���B��4㌑�p�Ap����6�9�8V#s��C>|��X��s>aw®��&�y��e�X��|2͚P��zx�K0�#�������wO1L���ly��=mD ^��ޥ3r����G�3�Tx��V�m ����K��M�n��X47 I$�w�m�^�,�����I"����ޱ���+xw�$Lp���i=��#��_7��酨2=�����e��}!}"8��e��Y#��ǐ������d7����Jx�6w�hd��r���*��C,�G�$�M��%�D�Z�q�4��m"{D�q���a����0����k�
�2G
��kW�����偣k'�����&e��m}{�ܵ ��
XE`+{���������[s�0e��&�aG�HN4�ս��M � ��
R]UTx��s�[���V�LQ�#F���2?m	�z.�����/���erυ:Q����M�A�H��k�ϔ�U*�u킏�U'D���u�Q��H�D�2u���p,�� d����`F�����`�Ϧ����U�YP�ޓ>��V���*�6�<km�i~l�5{a����Q�u��xm�R]��ԋ-2��09�9��u�{v�p���Ik��*���(˕oM!l2�Z��1�!�9��I�5G���DE��ȼ���8�Qhd+�(B��+��ji�:78QpYj 6a�f�X��x�2SRU��S!|k���l��,���D��Lש��s�td��+�t?�\��c����ep�]�����݇��jXj�@���� *R5�JP�}'���/�Ҙ���-��R�3���	��g�K)V�xe��D5&ޠ"�p������P��e{}b�a�"�̻���Ppׯ���I�F:7��s���!��[8��� <�੮��`9�;v�����vlu������WP'f�YdWAu�@f���* �2�J=r�6���B������A.-B�zaŘ:	9S�Q��$y�N\�����5�]�8�qB:X,A��Ybm˒u�^��py�s��_|؏�w���o��%�.�&�^��+����IeuU��K}��	�zB�]��yE81�p�ՠ~�Nd70lv>�O�W�����SGt�tfc\E%a�bX��9��6>�i�f�T��dYe^KV������Z\�r+ľl���jC�_���4�A��6`���4�"���k~@����M��jW���T{���&z��_m<���o��~[��l+E:��.���䑿����P~�#�c�>P�p��<k3��"�l��%G,[u�{�P'��}�Q:�}�׍c�K�%�_F͝���?b�3/EH�m��"U@��Hl��G�E�0�2�4[V2��jL$d���)Ce^��!v\�g+Q����h���'�v�⥰�X��2�j��|�ƨ�Q���!����8w��p�8�/���7�~7l�Y֐��JG̮(�I:X��O���̀&���|W�&dr1Ad��>��#�$���)�½������sm�IMw~��/2�̲"�;m*ʗ�la��%�(��pm� ��0eg)�Q���0��ȩ�S��;f(�F�,�0pI��2��if�9睍��´�1����X֗u�t�'�1'���`��$���P���p�ST��'��e%���9�*�Nz�
5k�G�K�"G:C�����{�i��T΍ * U���2���P�`!»hJ��
�v�S�P��ɽ'�.ǩ4�����e��݋>}���
�>��\��Y�#o_1��l��E4�s����dm�W\
�*׍�G�ÍW�.6i}0����AQ�D5j�pT�,q�=a(�D�1�ѣ��Uh�e)�p��$'[�S�c�2�Ɛ}�q��l�Z�ʬ:�Q�<�Be�\4�gH�&�����D:� ��Eu��(�~:��r*1�?����b��SE�lB�����Y������}һ��7�,��/?��,&�E��fE�_^��N�|�M���
��ZY�)���d�!TyN>�5�]!K����.���UKsk�pջ`�<�n��߃���P��ȕ�[.;��jR�K��G��������V+:5=E���#ͬ��W�@�|Կ�����c;�v��k���.nуk�eL��/�D��3B���b�83��P'B�S�7} 7���s���|}��5LjI���"�R\Y~`_�8$BF0���Ν;Ȳ1�Yց.��Yu�Z�oԯ����� @yū�!,0o��Ȓ5���=�ul�.��w��
۴Jǳ�8�	�Z��sdUj�t�d��d�zsE���a��K�C�iG�<��ˍ�,���;��"Y>U_�ύU�ݱW��ę�b��X��	�v2���_W�:f�-���4��+�!�Q��2�)���ʳ�o`' �r�,���>2�ʖ���M>/K����?������#�?����rY��*�xg�
�`��H(}�TcL�X"l/3CF��#7U��F��J�Tө��D��R�$e�Y���N' oy��Q�����b)ȇU�������:�8�8��7�C�J��sr���ْ��!]\n����1���� &VcB�z~DQM��e��o����C���7�`m&�4JB�)��C�S<b�6ѕ#�$����׈�6��BvF)�߱O�ԧ@��t�o�o߾=�[�n��B���o߾4�!TI�MF0�~b�#X���n��5o0IB��/��R��4�R�M�h�+V`��GԵkט[X\\�8���h�빃_5��!�B ��_�������DU�-���vC�bU)-�`���W�.����ۦ����-�D^0+�o��z'�|r̕�g�����Z��QF� 4bab� �u�K��Ĵ*} ���)xE@��7OY,�Y��Y��t�%S�R��k���|f���W�5�O��*�ĉq�}�5�"��88�`>�w�~��{�뤱n#���Q"�7L_y��V?(�m

c�7�
�ץ\��SH��0�#�s���~h������K�z��s֞����b	����"����_`���8����Q��͓N:)\&�Wޘ�����{*M  =���H$����'������K9�)������h`l�1_-^�>r���o��."��AizѢE1�J�Z�`��qS�x����!��J�Z����{�: h��ϭK��%K����70��k�.5�n�ҹ'�����>��	�A��x��ѝ������zR>V��:}�d
p�`޼yM�ff�5k����'�J��I���_j�(��0�%��,\�0��ɬd:��(@���[o���Z�E�t��;���u���W�t�V��g�}��k׆�8*�9��n݊ٳgG��G+�;�e��.�o��4*����Ο?�V��Ҧ���6.[�L�r��5�l��Y�f)H;h�_��O?�����F�� �D�e�7�|SK�>B
<��3ظq���G�FL ��p��'�|۶m��g�~P��>P�	�Q#\�1�(pmذ�?��r��i�^�R�F��\-�Fb�Zb�Rmz�嗱`�-����(�%Ĺs�*�W�Z
1��(p=��(,,l����$R`�ҥ�{r��4zYl\ 3!���m�݆��"�J�V� ��w�}��,��ф�f�d�_�5���?)�h
Lǉ��̙3�pk�JI`fB6��o��W_��L��L��{������?oV%
Wl� s>�'���>�ŋ�+#}/
Љ�W^W�(`E3��K`fFՉV�[o����!9�_����{�D�λ�қ���Gl����;�CE=��s[J�~�8Z_x�<�������>)#X�'A�H���ەIS��>�F��9s���P��x�e�I�j s$�駼�1P����#7�`�%`N����]w�b�Б��*���o~��
��Xd�D���t�L4�p�Y���J|��Wjc���eG�(��p����pO���{n�/�){uuu�BҬ� V���,�!�bȐ!	��u��L���k��F᪥���`8�s��p�Z�r|_�[n�w�}g�rD��myƌ�+��Ī�D�ę|K%�sV��2��׭[��ӧ�n���"��*
��\W��f�H.7�6�UY��r�W�HO�z"��=�:uJz�5�۞~s�W�^ˏ8��G�*�rKm1��6����BU�#�c�#�rF�]�J����-}�~��߫E�"��6	9��+��QʦG �����5��6U:�X��l �p8�}�p����j��`�*�m�y�Wb���I�ّ�К������P@M��fk͵���d+�H[�>O!O>4��)q6��E�T�������K�:*R�h�k�ː)�9ѫ�V��`����ypt��O�M�6#G�D�~��(��La��!|��'ʭ��n`5��{����̹����2e�����w{	ܙ��=��R)_�Jx�d	#��tn	$�5��ID�on)čI�9f�$��Ŗ����-[�@�M�U�J6��{bݰ&\�ɼ��Xts����^����T+JޣF�g���8�l
2$6;G���*����N��ձիW���Q2&��K[ͯѴ�8�S
`}��Z�s7n<x��{���W��,ٷ� �mjg-�6P��5��h���t�'������	ɣ����H��(�A�x6��"΍d�z�G(D8b������Π��#���o8~��C|���0�gM�9B�A���R��ap$@�Q��#�;���?�v!�1    IEND�B`�
```

### `src/shared/assets/logos/hoec.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>190</exif:PixelXDimension>
         <exif:PixelYDimension>190</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
�I�  2IDATx�]	�Tՙ�k��h����!��a3,a���qf����(�0NZ�YN �G���$�8��9J9g� �i0��qd���Y�����ﻯ^������W�.T��������������¢	~�؄�U��<����\{�dK����J~~�L�<Yn��&�>}����X���@uu�l۶M6m�$~���:uJrs�R�w˥��]l6h����v�$�f���y���u �J|��R\<P:u�T�0a�8�T���/8 �������RUY)>G����֛�RB?O�0��0&���U-N�G\N�̜5KJKKe��	H���9"k֬�-�ߑ�_j�  ���^#��S`59F��N�J���ү� y���e v�\mA���$9��� �7o�+���ӧD\��d�F��U�0Zc���̓���P�[)%%Ce�ڵ2q�D�9&��,��:t����=r�┸�6��e�5F�L��2�5hy�W�� ���(p��fŲ�	���I�dݺu
3bW�c�R�v�v��i�,���_~\��u�� V�l0Z][}��;�!��v��ng��mUҫWoy��'Q�iQ,�bq��=���ҷ�s�`g��UC[���K���99�.uѢEP��z �%�*�3g�̙3�Y2�� ��3a��ݮ=�^/���#�XC� ��{��,�����h�=b����[n�E����[*�����WY��!��{���s�M�!1�Sr���3f`� O��ƍ�!C��� ѱ9p���ʔ)S�~���ǎe=1-�s����x�֔�iq�K8�m0g�i��1����2U��z���� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�� N�"sG� 67~	�w&������d�=R	��ǌ�_}_"u2�ꫥ{�iN}�gZ�+Ξ�?��m�.��N�d=|W�y�Yٺe�x���r�}��5#G�>'������V6 �������Gr�{vv6<���*�9|X��Ӓ2x����;�w�%�;��S��gϜ��=$�}TCjs����vclh1�u#++K�x�t�����rϝw�g�~��r�zd�r�����O�8!/X ݸQ(��$���ҽ������Ν;[L��20���*%�;��$''�6ʱ	?}��<��_��/�0F-�5���|���ղ�o�l4�-	�(|L-�.��/�$+ӤM{��o�*�_{M)QA�"-`o@�`�.yڶ�'�CZk�.\�U+W�i�ğ2ؿ�s�Fga���!^�����?@�w��w3�CZ\SS#��)��M����2}7��������t���1�Qj������2=�5�tE�lŊ����˦�ߖR��T2Hn����1��TX��w�?ip"P8���9rM�3O?��kJ�����Ж�Tu�`j�u�n��vy���V.������Ɲ�עu�iњs$�l�٣��
2f�?g�N|��2[�V+d�Ԝ�=5�@k�Z	`*9���6�t:�d@4f��P�a
��d�p���P/՗.��#u�1��Po���߹��z�����>�uO��٨g�M�����J1�~]+��B�=�c�����N��� ����Z�`�ԟ�\���Od�o~#��N�3���� �~�,]�XNc�hРA�򷿕.]���
�S�U�HCf��?*�ˢB0�Is�L3�F9��s9y�8\MESy0Q ���ˇ!ր�GȠ�c�u�s�_SKGMՆ^����d%5j�<�d�|�{�,��@������z߉�����W�^�4���h��M\����K��O>�H��\�X�.~8�S~���Cb(	]���)s��J���w��@���@p��/�a�Ii��7@����(裡���lX1���o��r�YSh�iB�&�z�T����M��}8 -���GM�U��Y���` =.H/��N3%�e�/�:o"Z�X/I��&Φ 3f�
�,@Q޹�l��?Q�&�#8"�{Y4�z��D�Ǧ9�k79	3j|�{��8�_5�̃���G��t����,�0P��:�	Ơ۸��x�|9T��?�7�6��'�n��r�It(;�W���`��OT�"�2$Gp��}��
�8x��R|ͨ ��HOǧ���qAi
*W �@]U\,����y�䱢0N��\�G����R\e��W���u��-��IU�����K�T]8�4�ES�7��&�k�~����	Vc�e�XqFYOƈ�D{��={�ͳ^��S���/ϰ�W�-W��A��|�SOI�~�),�y�x3`a�Z|�r�Qu���9ԑYuX���>&��|�<�`>9�1S��P���p��|�J��.ڻ��p��ϒEH�	j2ک�>�@��ٙ�~��\m�:c��0�	�Ǚ�t2��`#w�qN	Ɇ��5/���Q�KL$ˍ����)?�eoo�kF�h�}�áV���V�Z��!O��9�f;^���j�П��U�)���Ϣ���$���/��o�F��X�&���Fd�f7�`'�r�S+��g��� ����.'���|�*��?���al���U��矊��&"��g�>F�����Zy��Xf���-*��C^a�ڞŃ`�k�ò�w^>�F��x��)�=i����|RW�?�����r'�Ϙw��X�N��ms3� �f���A3 w"�A����.�V��l§͛߬|���X�G͔�A��V�	�)//�����[��C������5�o~-v�[Rx?��1�F�b؎K�Ğ��t��|�m�x����6J|���xA�@[Źsr멍�F����Ʀ�Hm����4�}�/V��U2�� 2���_�����+k���,FŤ�;R���]
�t�n:�}e��*��҄yq��̴����e��h�YSBq�֭!Æ�s/�W�o����Z`)�Ƣ���gE#Fʜ_- ��@�g���h 3�ƈ?� l㑳Y|���f�n�/C��N���|H�V�tʺ;�.Z`7����{�`����Ѭj�	%;�$����R���Il�zh��Us�3�����4���nZ$`�Y�T�����~85�8�r�z���X����yd̦W^��_մZC~��N_�F{�7�DFL�&'�]}C*����Z���`��/��~C���g0D��%"��'TК��t�r#� J�Y��?��<�}@T��}0��`�cX�m�ЈISe�+���r¼����������X�s���z�6v��u��y����T�C**�s�y!҄��I\��,x�)�U�(���r�j^�5����~hӉ�Uzh@*�l,���M\)�tY9y2��n�JHk�^�2S�N�q����#��菈Ԏ7R�L��4M���s��84b���:���J����$�&���*f���8f��~��6���ƫ����j�R�(I��@1(+�؀Ъx��z<�5�m��\�8`CV�$ɟ	M>U��l7�#�H�^��=�/��O-Wf�x�AO��cY����(}�$�̄1��dx5��{e��n�3ǎ�1m2i��)s�re���r�9'�G�Hq*m�$�y���Q`������{���T^{�1�Ƽ�j��2���r���C'Qk�GN�^Fc�m�`7��t�N�)�>�-,�؛~�U߃�I1A������'!W �f�<���?lxR�\h/��>"��Ko�8�a��ˀc��J��[)lݓ�ؠ���!@�L�k��z�9ٳs;��ğ"4$;Ů	LC2/Z�r
:�=�T�4.y�	�dǋa�8�1��V���.�jk��?_���0��%%%��ڵ1g��w��z��)"ï��r�1g�r��r���eX��VÛ�x����Y�1)qゅ2��ISq����D��#��Kf�J�x��">�Z�};�f�߳h�<�ҟ些v4%΄?��rшk�W^/ߟ���b'ͩlR���td΢��5^�Yץ�Y�\a��K0�Ǹ��(�t=n2�s�t���8I6�i���+C0��.n>��d3ʱta�^2�~&�x�QT�G�{�i�E��\�D��?<�~�ǔ��N����+ �������,�++[���h�7��?�A��خ0ϥ��f^����΢Ywߏ��7�$�;M�~����g!
��{8��_V|I/^�}���t}�~�L�~�J����*l��g����\��#FDh�����)�����3��ԡӕ�f�0��47 :1���hZR�WFd���+vB�e����eߗ_jJS���.&��6O�Kk2�Sp2/���W�j�rv�7Y g8���΁/�%��΁/�%��΁/�%��΁/�%��΁/�%��΁/�%��΁/�%��΁/�%��0=�[!39@l���	t+d&����7�lr(���찥z�wľc����;,2�����"����#[�f��>SK���E,ׯ_/���e{����>q�DcC��رc�a���1{M�]�b��U���80�V�g�a���J���6{e-}>:e��-�e��[�� �[�l�8�\��{�q����e%v���cV0'�}�]Y�|��Ju� !�쐫ᚷ��D?\.?��>|؜%��T�e˖)�֞�x�2cǫ�t _��	�{"����_��V0�1;r�xm�R,����]Tץ܉�×I�)��]r�|�����2p�@)**2G);(��R,X G ����_��`�-�W�07$C����/��N�p�,@�%}��Q@7�_T�u����M����T8,�ٲ�\&A��VX��D������/ˇ]��9s�,\�0��b۴t�e��i����l����:�b5]KPj�%��F��~J/�(�@���*7[ 2\�C��}�[o�Un��6��X�m9@�K������2e����[��<u��"������v�#�}f �S~1����8�턶/n�{���c���ٳe����Z¸Vhpʏ�FN
��G���}G~уaPU\D�� 	A�JHE�9>:    IEND�B`�
```

### `src/shared/assets/logos/igil.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>150</exif:PixelXDimension>
         <exif:PixelYDimension>150</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
��Q�   �IDATx�pT���B]�����`��q����`'��Ğd<)N�t�9�L
i�괙8��0�q�ĸ�16��B����z		՜��'����V��Yv���}����~�}YYYCr	��А
����[���0_EFF
�����NĿ�'b�G� ����k``@���$..NRRR$))I���%66V&M�$��n�s������WΞ=+���r��i���6�����k"҅��`�H ܴ�4�;w�̙3G�O�.������t�2e��'O�l@��`{zz�ܹs���!mmm���l�noo���jٷo�>|XN�:e$�����0dED�Ep�̙3��o�,^�Xrss%33Ӽ 1P�jll4�744Ȟ={��_�����c�u��Pzi�
���EEEr�m�`�M�&�����`,�A���/���<���l�o��`�Û�C`=��5k�\y�����߈`onn,������k�����?#���ꌾ������h�)��vчr���M7�$K�,������2�#iv��)/���<�䓆�1����I!���`��w�!���3�c��9b���5k���޴3^@�+��b,�U�V��?�q#�'*����a�������?R[[k|�v��`D1��ҥK�3���1��t)}����(�֭3~6F���>�`�1�
������|��&�����7m�$�<���ʵS�[�_��< �=�c~}���\~�a!�lk{L �k�������/~Q����v�G7D.�x ������޽{M��e�E��:����py~��<ֆF0�ϗ��%%%�b�
imm�cǎ�xx0@*3SA��G�K_����2 ��9������7�񍠈�q0��P�W��U���?=!��1��f,0����ۍ�HN
�X�Ċ�����}��wY�[O'��ٳe����k�.����p�7//O~��_�ʕ+����t C�8�
�c���'O$-�@)Q)�����d���<�!�7����>*�^{�1N�c���q0�rqq���-[�O�.�s�BY�h�q�(4�G'���\��~����.{�1 H����2o�<��M�9�%�G������V�HYY���曦�ė��_�~ p����;�����Y��� CX��$���0�_��'l�����Q���-[���M&�g����塇2Պ�q��5�S�����]�2U��ܷO .���_���d���c}�pD1�ozJ^�QE��k_��q�=�P�8�G������=����X{0��zd�*ot���n�@-R��ܑW �@�:u��a=�i�G��W\q�G��W 3c��~Y�p���Y���#���O~�&�ꎋ=Ê��?����?��`QS�TuEL#�����>&��ɮ��6F#��F�H�{0f9��DU�:#@��vWn�[�q�X�u�=��KnB[�b��B����x&Ct�UW9;?��8� �1|G�.�$t�{���p��89�����E���Y2�-�,��G���]�Jg�`�3�x���u,���#@D��;�t��
0�R<w���{~���6������G�g��/��#@�����`f����ė2���K����"z;;�`|*�N@y��T������9,BW��	��#��ť�[�Nf`o��F���>9�, ��k���mr��Y���=�7H��N�8�W*�8_�rst�&b{�鱙�A���0���%:H�L������}���7o���L�E���.=]�R{�Bڛ�OW�WW������ǥ��Lr�K�7I�[�Χ n-�qKK���v ��$N3Y iHۭ:zP��z%fR��ٲQ�Sӌ������3g$5#KS��О�TW#�I��ٻS��J��Z�q�3fKzf�K �{�ۢ��k��'�x��Q ���ǁ� &L[K� péj9~`��gO���$��ҡ��$22"R�RT'�H�r�`��*G�gf+g7H�����(31�m�Ň�K�GO�`C���=��9�`Dƕ��`���\�Y�����so��	��v���+�)YS�I��F�
�s��4�ާ�ޣ�U�<z@Ψ8OM˔�#�e�ӏɍw�+E3��gR�[�tL.��O���s.� ��
7���ZΞnU@���ѿs�I^�tIV��ItL��q���d���ѓ%N�Js�)��~�im��ړҫ::A��i]��!�D��L׶Ƨ��q�0e�����,������RM�k��*#;;[�u�)����K��zd��urV����(IJN��
l^a�LV�TOV#����=Z����m�=��Y)iY2��;%3K2r�ҫ����I��zSٳu���2o�52m�,���~�+��k���S4%J`'E�=!&$K<��l��Ͳc���K{t����ɖ��Zp�ƉY5���)�\�.#�E��r�-� ���k���,�+����Ҥ`uH����ZIRcjւ+T��J�T���u'+e��V:Z��cx�gM�H��C=��y�74)צ*������	�*I��,u�'̹��&��)�;9��?�������^�RUKת�	�gt���������RQQ!�j�vuuɀ�n�(�^�N˲�?������=������k��.�Ө"��ɹ ӈ?���77�����ʡ�ƀ*����+4"˙���@���.)�1G���?*J�.�ۥ� V'۠>)�.u���#��ɚV(�{�����:5�N����iť��n�w�K{{���]w�e@�+�Y��VH � �cx�i�o�aR����g�z)�:a�t�j(V?ۃ��i���5��`���BD��� ���\4�F�q΢���V3����*���.�2��D�m����-}�E�z,�����<E��:å�u��4����v�ڵŜ�5���y�]��>��-\�I䎶m�f��({0�Օx���Lv�a�¦���c����{�I��S��mO�6��i�`֥�BX�5'���E��4X�%��X�S:M����]J��1��2��GD��<0��3E�W��M��<#����$�����e�M�R�ޭ����VJf�u���KҫzL�g���|E^}���s�M��n0\�F,�_���و���^3�M�!&�SO=e<�իWms�3f���x��o}`Y�/D�*F�X�Edf��X. �W�ܯ@�I��@��*C�q�*�-:?����'��L�\5�p��4��5N?���KJZ�{�׍��-(�`I�դW�v�����e����뇿&V�}H>��əZCo����gOi���� �6���Ý�}��#�����ܦ�S��Et,����9����퓃��I������ n{�E3	�Kf��V�V�	r*/G����������p�����T�����u����vO�����[��m*�j��� �T�k�t
�Y�N1��G�����7�����Y����7�[���������Bđ�m{��ǜ�"���(�Sԇ�����Qn�R�k����͚�HZ�7ǩUL2*�M�|��{�ա���pD�.=��l�1ت+�J�rr���0�7�G�O}t��f���y���81�F�}�2[p�>��|�Ϟ�	�GK�������LBԅc$xK��)�*��T�K��X��b��HP���O.Yr�Mrx�vI��T�&Jj��x�z�Ƙh��z������y�>k�y�n�����VC���s��
��AÚjХi�����}���+���~cs6v�8g��<%KZxz���,����譸 �z�S+�4��pJ�L)�ŉP=ګܥD��SGK�&I{k������1�f(0���s%M_�
n��#\)7����)i���P��>g����֬V|���,����>�ƽ��G�<��3����U�����4��� �-Y�`_���Qf�
"�i�p�(����z���Y(G��Pp���QEV��bUާ��~��A��=�:w�b��ܩ<d��W�y��[�p�\=?�rӊg� ��x�<�@��VW�[�����؏'��s,�JF�5��/�p������̔\�����i��s�F�v��mi�7�P��\�d�-���~�z�5֬|)'+K��q|d���j���E�vv���G�U׫�-0�TBR��.���g&F�Z�@s�/<b�=�A<�G�X���j�1��~L����{��Ř,ѩ�]�rn\|�1~YZ�c	�-�f��n��7�T��=��.�����7� jb�iP��Fo��-bv�non2��C��i�ƅ":���ш �+��q�q����~�<��&�6�xE�Z�B��@0�`_�;������! p�`�4��΍�����N¿��&��������B��*�j4J}��8�h�r�g]��[75�h9I�J0�erq���2WD �юi뭃,@\��o���f�.8j�a�P��o~��a>��Wg�yK���͕�;�l�x:v�`t11��NX��/3\�y���T���q3��ZE6�v�Kl��<���L=��W"�֚zߞ�����F�%J��"�9�@�^<�� zB���L��"�\WU!X�j�"&)�����Rg�AU���l��Zk&�T��"5�fͿBk�.����
�>3D�
�@$��h�V7`�yB#���e��q[�Q9D�\�R�Ğ��"0M���H�K���y�7��psqf�7�X���-(�j�.�P�hi����]i2�KJ0I�#C��.��TnXu�l}���+e�rm���KݚqJR�]�	g����^�?��@KrZ�����t�;镘�9��������\�M����o}I�;����v;PH&��������/���������&1K�!n�3���D,ww�1q�T�G��u���3�����A�/�"�f,�F��֟�6)Br�#	P��;�0�bTߓ6$c��E{S4�4]ۢ?�(��\"��[o�B���Q)�P?Ī���z�]MM����Č��4���v �ͨ��'` ��EZ�V�p7��u�4S����g���cG���Z���_T"�'OHs]����Y��>l?�|���6e<�\*D�t���rlcm�	a�*.Zs��;���0���҂���b��������î�}�{�[��%��W�<��>: Lb�����ko�d���Nj�Ck��TTf�&��ok'��ڭ:�c�޼I
JgK��uf�>��*գB�D4���1!'�~�p �m��2כ��}�U��8ܚ��?�m�qZnӚ5k�o��E.����o}�[&}hK�	�ݑ/ c+�W�Q`��B�j�෶��T����G�t�R����EİI$PV�X[mB��ؠ�	`ףt����z��	
B�D��Zꪹ�$�d�"�]rG�����?K3�/_>��`��G?2���N>`uCo�t�,��>�9)..vr��_�8q� |��E�Do���J�9�|?�E��2�
F��;�Q&�����G��O����&:U0��|�"�o��rp�Vy텧4��.�\k,@'�a���b��k|^�/��"E�%1l�p.�$�wwd���B�O$(���c�n���7R�̸a�R����wҍ[�n5F�"(۹뮻���>�V4������J ,(��ٟy��L�N��M�'�Kь2�U�%^u%@���-7��kn��TKRN%��E��Y��L#T	���8���R'�j)����:9��&'r4(R���Z�Yh�5�I�����~�+���f���`�.���D����5� ̤`L-�w
H�D(�=��:C	�� 39��x�$�i|�(%�p&Vn^��jJDj���h�CQ��a�N��c��lڅ���D��������7��%KE!��#*��Q�����g������e���?�_�����cX4/wDN�T#[�����7O%��҅g$").�������|��j1/_�n�Sv���i��QWnT�	]��nT��S_h{H�ۚLy-�M]4I
��ʯ4>.�Ѐˍ��m�񺜪<fٚ�X�^��P{C����q:њL��`�0�&"�*��Ǐnr��x�Q��^�䅉�Yj��=��o`�� � |=�sVgd52�;�&M���d	1fjП��F�SX�����DsT�Ʊ�#I�8�D�#��z�YA���F�N�GW1��n7�m�5&��~P}#S�]r������w�c���2��V���A�m��fd����>��mz����(Fl��?wΜ9B)�+)a�6��%�A��z�� ?��?�gb��1� *}%�ݚ���^P��U���>3�Hr�yť�y�G�:���EMF��(-�UWQi�d¡��4՝��-[�z<�p��u�Fr���o� 0f<*��IN��$\���}��i��	���|:�d�	:C8w��Y���F�F��a������r#�����7�B���Ĝ/1�3��3@S��q]�n��:v>!�~5wd�R�25�;S�|q�����#���#
Y0��ʃ�m8��׾�D��'�a��a" T�Zܗ�G��yp���6��EߧU�ej8�\O�ѧ��j�F�4�c}�(���(ܜ��c��)�!��D)Ԁȼ%�$Y����ո�eOp<>7~���F`�7��������Y)GOՄ���ʌy�&OL��W�fVv�;4�b�Z+�9Q+4�����r���%�^����a�0x@/����-���K�U��Y��>k��@D�X�۠��h-�-)[���#�h��j�Ҳ����^��O�u1��7���q��YI�`�TV߱lў��~��я��pH���X�p"�MR������LT
��:k�Rb�P���<���7��#G9X�֩�s|C�4����7bE���Op{/��^�]�/X��h�R��>YG�������i��q���]�v�/���d�Uu�5�Y�0���5���ch�Q�b����E�9����+�D��+�9��[�)tG`����(��f��.9�1������uR�=tF�Xub,*wV��`n��H�>���{�o�>��-o�`����?���6�C:#@đMd*++�r/=u0��w�Z��wa�`�
����y0'���h���� ���~�3��r��&:B)�o~�W����_�j�;���]�`Nd�-��0���Ї��=��{G��+�i�T��?l�TF6�;�# �VG�հ���+�9�L	ev�Yao8�98#@蘱�-yU���y1����4v#��D+,w���� s"��ڟ|Pwk�k�~�0��Rc��Ӂ�.��4����^�z����.���@�����f�j>L�1)�a��0g�򗿘-��Y�ή��\���ֺt��w�;�=���rUO���QZ��������(�'S�Np��4o�{9O��<`�S��Yys�~��,�Y����a��A�ɱ���Ť}�f02�6˶8�����P��R �.W�4�DZ�l�b��`=N���6�C=d�yk1;�R@���#���f5��Jg��ܿ�ӟ�d�rEEE@J�πL���&��:U/Jp��|Pm�;f� b���\�JA����-�vax��ٸ��K��b����#��hi4�w�����c�?�d�� �}��,0�@Ө+}!��eپ 1��&t���	��G}Ԕ%�*����c0Ed��<�����eg��4�}*ѵD�<��tBLg�IlB�&`����{!>�&�������[q<�1�`;
 �Nl�Ql+�j�* �s)|�7m�d�]�n]�t�hc5n [B7c9�Q	�C��{L����D}GJ����B��TL];�8�;�tnF�� ��\��+W���G�x�~�S��F�g Xı7I�@�[H l�@[:C��ـ%%%��X�
�;�֐�g�B��`�M���i/� �w����sv`�{@�����ne�8�.85��
��x�9d�n��	uIii�����#hB䉍�H籚��1{d�_C	Xk�B`�������ȓ}�/^l ��<+��Dx���`Ra ����}�� �ݣ�<%@Fʺo�'���t��;�����2ݞp�y���1 ��v.C: ���>�]v�A�� �>�S(�V{\}�� �vC X� ���9���D��Mt=�9����P�������%��������(d���>;��R�1�=X�Y���.�����؜���    IEND�B`�
```

### `src/shared/assets/logos/jindrill.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>190</exif:PixelXDimension>
         <exif:PixelYDimension>190</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
�I�  IDATx�]il\U��ޫ}�]޷8��=dq� �4fi �4�z4B�B�H��?�j`����i5���%q ��8����^�ګ�;��N=C���8����{�w�r�}u�V0eEZ��S��ne�=����W"�f�a��ذa֯_����QY�����4�����G᫯�Bkk+�N'4݁�Db);tI��OAJdX�kZM�Y�2�=�-$FPTT�U�Va�ڵX�r%���e��XI���[�l��ﾋ�����BL�!�t#MU5���&���eN�
Ђ17^}�G*�y���_�ҥK���X�c=�'$��ߏ��{����c`pI[�	�it��P4�G �5���.��r᷿�7<���(,,���Y�o�:��^{{������d�#A�mh��]0i�Օ��G�J\����
\��su1�xH���˗/G˹shmi��	��$S6�]Zp�L]5��a����o�����aU��$PSS���zk׭G,2��0$v���6����*m���o���nl}7~$ �����b�u	�N���I:E��	��$�lD�aǳ�>�իW���[5�E�^�k���;�X���y�8C{��r�>�O>��q���d�7mڄ�I�p�CХ���#��AL�1O?�t�O|��fU7+��S�⩧�b��kZ
�R��q<��3֨�I��ƍ�o�>�n[�DXd������L�b�[��kx�'��Gs`�㢡�v;"���5kr_X;摀��]�x1d`�"�I@O$X�p!UY�/-2�t�@�5k������J@���(..�bR	��{-�M�.�R\PP`^'8g��h���0m3�M6���	Θ������%`r�,� 6�LΞ���&���ٳ4���09{�[ �\&g��``�K���Yllr	��=K�-�M.��gi���%`r�,� 6�LΞ���&���ٳ4���09{�[ �\&go���?�L_ۍ�� ��&0��
w�X�03�}x�&��O!N�n�~�[~�T2��@\�a��%�\X�����\>��ĚYw�2P���>�:�i{
6)��~�Tf��Tbv�Lx]?N�:<��#_1's�i�Ӽ�f����Z,�>/�<R��8��C]В��&aŴ�L�-9�o�����q�y?�K�O&��uk��떃8�qu�2�t*��X���^��Brn'��D�xd�? �a��/�-q�������Zn=��Ϡ���	�R����4�)|}x
���o�}X0y�h�5g;[���23&ٵ�6��p�j�����m����^T���u��u.�AUQ���q��h��ek0wrF+�{�V����7��ܾ3�&�'ԋS�7#���ߏ;j���������F�ӇEշ`��53�ױ֣����ƿ���(�]���g��`��F�mN̬��������5���W�{���j��w���3se��,��@o�v;��J7��M���5ĒI%��H<A+�3��� Q&(��&ɟ�1u���QU����<���3l���9QU�DG�e0�xI�h�fT��÷=�O�~�z����~8ࢩ��*�%�c��D��:Eٸ`��`����0?�_��%7 Id�Q���M`\��
qͫ�E�~��a0�[*�b�Fⱨ���m�e���voƖ=[�y��moQ��,�Ĝi�p��qu�E�"?�Լ.�$�/8�c�wR���hG���zJ�J�)�Tb}�َ̍��,�|J�l��].BB���Ia˪v�҄:�>t��+���awbf-H�%��x!�Ù�,=F�/�%3�`zy->��c9�uA�-��P�H�xQ>�as �J�_�]C=�;�Í0k��PDlG'JH1P�G�)4���`�ז��聲nD��`�La	I�E7�s�_&���T�F$��Z�Ξ.L9m:�R�lj�T�Tc��yxp�|�|��{Oת��'��	p�zv�Vd(9��`�:�0�Χ?��+�ƙ�n9vҵ���M�ȍ�7 �\cY3D�Y;$���r��̲́�lc�.�pxHɮ��}����I����2u_�B�r!�*�VG@]�d����g��kB��2A+��eת"P�J��XX=#�au,u���9���>]B�hantI�2� &�YM�i^�ʇQ�3+�[�t���O�2J3��]`��wQcS4��{2>U��T\E��tq2�rif]��17^�X�{�>p0���B�'�
]�F���]9��v:��F!�`c�R���ɤ����1�G ��Q>���]��d�$$�i� ��@�f�9rm���i�Q�?Y��iʍ@G\@�I�?�W�a���)n���DC4�v��
�B����G��J�F�{� �����	�<�d/uB��q0Ț]C��W@F�
9'I�#��� �?u5
qPC��pZv���eG�>Ո�%�}�A�h���	?-$8+r�s�^�N�Si���VC�J�E-�6���4��a�j��k���q�Ҫl���=�H�I_�0��w�*Mӳ�E|k����ƸJ�� GHj����
��+͔��O�K �y�s��̲�p>�3�������q���y3�%>�ZD�9���{��ʺ�h7#Y���V�n���I�hV��)�G��))�������ׁ����RD̴P��7�6�����<N��ųjt����*�������v��G�zh�:Z�uơ�:�F���e��3���7��D(}��8��S��~�hJ��0I�*]%!1�N��jf���t�n��헑2#e������l� �z<�v#*��EGϟ�����He%ƍ���~z ��n$̮����8�����`Q��SZ�d�����y�����f�.S�3F�h<݄�� e�֫�#�t��	�PU~^�����r������;8�����3���x���0�� Ȕ��X6g������d$]Ĭ���c��>a�C�x�5X��
�;�a��r�,I-N��?����>ׇ���y�]����X���r���;M�md�0����8���&��R+��Ԉ5��FraN����e >B����P_Y����<��(;I�`G���D����'�\� ++$>{�h�48�%j�z���vs�`,X��eu���K�����*ib��\< =���3�)�����cy�x�0����&/*/,��vb(��i���NJ��
��!4݉��)~�@6��v���m���st��:Zq��<��Tk���9������|�S����T��죅�1)p�l�
�����?�]'�ȗs��p��U�{^��1x��M�dK�` 4�.�M�0���T�K���;q��~d����g"��G}�t��_�g� IL���i����T�L*91�f�z�,N��x��T!��A	*��&��.S'*Y� ٠�"P���)�n�d�b֭�k�YU�8x�<���O�P_V��Tޕ/ �u��8�����1�j*�_��А�V9��*�u��v� �k�^Z��5s��߃��L��`�1G�2�X�23#\�)�=s�1��wͻ=�o�HØ[7�8�b[ř"��K���b���~�hY^	��]s����T�g�fw�jK��\��e�%8}�E��9bn\r]�c���Tfv����t�	�������f�10AC]f�\u��0ߏ}�|����ʷ����������3�����h_�i�qˑm(��E*�Ajx0�9�g���0����������ULP�K1'���@=�n%*���Ò�+k#RFL���ҟ�'���p�E�4�����AZ������׃���P��}H1�6�E�+ѿ� ��p�� ���d.��?w��u!�Ӌ�Kpr݊P�yx���d 嫯C����:��gQ��f��%D��j9{��i�(��v��C@5�y�p��݄�sל���IÔ J�J����ص�Vf\�km�`?vK�K�ѧ�I�6�cN�I�u~�	��'�vvE� <!�d1/#����p��"�Lt�hD`�BO�D����R9p��v�U�Acd,�;
�z�tmߡ ���P�8���K�d_�~B=�h�nN�:�������n��a���XѢEj�|9���"ܞwm3�RS2J�ce\�km�`�0�d�q����� �[��%Pz�*,�WW�&�m��h]�z�2��b��x�08��%	|ZLc_�z�h���f���+����ah�N�)�����!I��ގ@>��N�୯E�Z-&U����sf�9�1t�"��HAs�8���2�ͯ����I��Oc������b�T:5�OL��i��E��jHPވ����o�C<D�o�%`	D;�lm�ps���=���ZL����8����~���]�iwy���PK+5�(�ѱu
dޢL��X����ի$�c�҈�5�h����K�������8�M�7ж��H=��/�ap��\�7��or��'��[�Jc��s#*�+2��B|��OpqD�.�9�$VS�a�<�?�9g%|�W���a8�*���Y4E����ҩn���L�x�v/����΢�2���I�GA>�}�1��O���={Q��V�nmUZ�3Bg��[H�� B-,N.��7{&���A>�� �B��:h��J|���[Z��5�G�G�QMr�Mc��)�&��d�%*�`כ�6�?���������M�T�8�ʍ�-.�Êߑ���]x���%�%��R�l��Wj��c���^썙H�eV��y�ј�E�7��>�&G����09{�[ �\&g��``�K���Yllr	��=K�-�M.��gi���%`r�,� 6�LΞ���&���ٳ4���09{�[ �\&g��`�������ec
���<c�?�Ȝ�%�c����Ȝ�s�?��Ȝ�{zzp��9srgq����N��DaR	���v<xP%1)��-�ɜ���G{{���Y��\ĩ���7o6+��/}0�b���b���#q�z����i;ZZ�b߾}��n�s"�����+�%��H�k0��믿n�b�4�>� G�er���ƹ�sX57���o�mE�79�-����;�(.����0�L��1ע �e˖��ŉ[}������,�Q&S犸���I�(I���ZV�|�嗱s�Ή+���s	�_}�U8�����2y��[�I�u�&3s ���1���sLfZ�3fܤ�N�jˌ���?�m���̶L��\l�� +C��>�Q�%���k�.������abI�&�v��Z��>�Np�\����t̒o���nR�� 3�5�.Y�W�_�����&{�.Т�%�C�ᥗ^TF�p�Yp����xj&���n.�q�\�\�Jb��x��|����4�ՉU� �J�����C48�9�TKRnQU��XjޒiL���"�`�E�����e׸���j�u�����m�];����s+}�={��c��cǎ�K�u�̲��
��Õ�����#����a���h�9��<\;�y����*~y�ڵX�~=*+�Z����,����	[�nŶm��͌�N�̎�4��x��ɝ�
j`�?��1�9��    IEND�B`�
```

### `src/shared/assets/logos/jublingrea.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>150</exif:PixelXDimension>
         <exif:PixelYDimension>150</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
��Q�  '@IDATx�]�]U�]��N�&��Lz�S(RԈ�4�{��}��S�P�"OA0�@HBڤO�L������w�̝�L�S`v��{��s���_���>�%""���cC�������諺)��͝���$���̧��U7��c.�XWWW����ֹ�:n�~l ����ŀ��닰�0���#**�lk�>���/�^__���ZTWW���������AYY����\�?�ڄ��F�����2e
�̙��퀀 �l6����>>>���0�<�[@�SWWg������Ʈ]�������B#��,η���.E[�)�����qc�ܹ�����wPP��L�:���r.�裏��ob���fh2hl�x��:x�,�jnn6bV\j��|�rxzz�$��꫼�7n�o����L9rUUU���$�X�	��B�.Z�����q��b�����s���u+++�u�V�y��Wq��!3α��q�ġ�%R��_��Ԥ����:��iR=z�����?���ӝָ�i4ڸX��[e�Ξ=��z+V�Ze�� �3�!��_���|�I�8q���Ft;�q�劈c���z|���5֯3�>}�0�ӟ���{��i�g�\�j�
`�����[n��]w����u�c��S�����駟F^^�1Ɯ1�� �Q,��:S�ҳ�6ŭ��{/.������1��bٲeX�d�q��?n|����d��U�ߨ,`i�:u*x��k###G��cz-Y�qqq���K��:x�q����3���\�_��W��Kq,����t�Ry���6�Q�g$@�	�x�ҵ			����m���6�%Xù��+0c�ci?������p��y��E�����W]uU������$�����¯�y744��{r���#�m%�.\������袋>I��^Ǿ������-��Ts
�WQ+~���b���#5ޏm?2�V�^�G}���3�Ztnq�5(}n��&<��c�6m�p���:_���_
ъI��l��v�Z���?DHH�'
���Y������r-��F`�u������XG��C�����C=d�.���#��eR��sBCCz�ǝ������Ɂ��J�uqEg�
EGGe���C�!��׿6��P8y� �s�� ����sr�p(�����}��kt` ���뮻Lw871y�)�l�7��MS�5�������|�\s͙G7��) ?�k_���#���*�)�<�F��x���M�F�H�һ�f��w�a�lr��cF�V�&88�B�����U����o�̙3�����N����+��r@Q�A��م^h
Ν0��.@e��`)))g��A,{�wN�� ��C�
e����:R����m�)�Z�5k֘uU��f� K4k�|��_���LU��|��f���`�g��~����"�xۧP�������T�%�K���W_=�+���[��\���J4�֢���ߍ`���?W7x���͇ox{��ݯ�{ﶯ��쳑���՟�M�J�p����7�ݸ�L�.��(�bq�ot��t���b����fmsO�|V��[�PYxx�Cף��D@+�އ��Ro�Jb�����?'ACsj���~������I��K�-��ړȫ>h@�r���#^���W/�%8M�n6Ns��HTB�Ά�N�w^x�^�t�m���;�>}:T8��u"$���#IpO��,���O��c�/�Zȕͭ��S���Uu�f8��%�"����C�o����|wW���DP��5�yؗ����?��C�_���`���b�1o�����ۨ����Y�:-݌���v�h���֊rd�-�b��a��LP�AAn�#՚[���Ɯ*}ȖV�9nk@t�,xy�@x�K�j��'���(��J����g�#W�IӦ�pv��W�#�O%>��~�!o�k��M���˰R�͍7�8��t��n(�9bt`��+�\�؆ږv{� ���Q�U�\�Ɂ�$D���v�{{6 ����#�ϩ���Ո	�ͱ�4�����_up�6m8��V�+�<��>+�/p3U�{�M�WI}K�*���\�M�?·�~��v���ǯ'��(�ｏ��jƫaW���ډƦ@�|�4� ��h�BMS)B|�t�v��R����\����u��>��n3�:q,�C|��7�EL_��Vwl*nBVu.��#�h*,�gK+�	��H?�?7s�Cd��}�|�R�fԕn��O4<B��A�'�n�=\�f��|'�E������I7��#9�`s2�z,���5�����kM��:�_�%�������m�4y�p�l;�B��wdն�0�*�" 1��^�kjFNv�/Gz���aقx����Z����������|&݄��l�y�lz~Q[w'�K�,am��� �7�VU����,�㐱%]��-:W�w��Ľ
�9s�r����F�V�nV�VL]�_w4��N�V���!�q����)Q�K��Gl
�����a��4#5�������D~�'����v��Ex��l����}��%���,J.`��X5�H:�#tAq�Q�Ĕ���F�9�NKa�4w'c���ke��X�r�Y�َ���v��t�I�n,F������͟���ɨ�-Aю�t������@�L��׼4<�v6^{��p�ۀ%S�a�H�(DE�������9�V�{��s2鳊�x�$/7���ʫ�@�w,��t�A�~�������9J��R���ɝ��_�0��H�R;�iT��{!b&97!;��do�X���*?!�1pI��So�#�X�A�������ذo6�W�c������B�Plߒ���98g�R��]n|n�]s�
��l����8�"Q�vZ�}���t�+��� G��T���ԇ�h�1���5M(�[�����t_�
�`���מ����q�2��Z���$T����]
�E�����݀�K�1=�bR
d�[eC�l	���v��o޹��X�Y�����"���MjBY]6���
t����`��Q�W���j�̕�1*Hb�G��x����8����K
���C	�1�v?EsU��_�3�3�5���T0��n;��}���\���pb�o��î.�X�k�mX�"�s������ٍ�SŴ��Ҫ���PZ��E���zӨ�m�u�o�&���_���TW��_ލ{��PY�K�;��g�_� ��6����ܢf��b����λ87�7��c��h�'�����u�bŨ��i�tGy��*�a��FE{C��8v(�(�g�9���z̊���s���������`,:�����G/��8V�h<�_Gb�$0�SI����� ��>�d��`��F� 8-����K��M����U�/���v7%�;�LG��,̙[�91�a��ʎT���RYeu'0���r�x���nF���O��4���e�:�Y�І��h��G�|4�T3
%�LQ���fvQ�Jaj���I���>+�FU%�$~�b����a����M�Jja��^ +�;�y߁1>xJj�HV.�\��s��'�.�߸P�|p M�u��Vs+|�C1�¥Խ�سg*��X�{��bq��%Zە�p�\�@���̔�O�~���^=�l��_�����*�[�!�C�X�ؘ Ĭ�?o�䗳4�-���ګV���؛�����qβJ,N��#��	�r̵���9Xc�߽b�X*�͝� F���h ��yv��5y�_�j�s��rE(unR��31/�[��0k�2�aI�zF�.5E }�s}m���������`q�ҥ�L�&�R)��]��?�ŝ���Dz�";���1�`sA���I��5�/�.\��Y1W0i�䌜�-�d�{M�'�uX��hk"UNz��Xѻ���;#L4�����4Y��Y��f>���J�� ).+��*R�#X�eL1N�sl-,��E��س�X����2G��)�>q���©�m�����z��rA��F�1�Y�V�kŦ���?|1+$]YY�z��Kv������*��o)�[�Y>���Dm� V�z,߅0XBz��!5r�罌��������*��� }=C���L�)�#�J=�r˕@�3C
Ѷ����H�+����|<�}�_��	�c�V�g��T�0n=Kw�MxQiP���@̳��j�K96ڈm{�ޅ}�J���O�'޽�@��^?9��kbN�7����n~�X�S�8��6N�h\é70��{q� ��C�Z��H1*X���WX��
��0}j��e[RV���3�<0cJ���yl�+���b~dL�z�#����h�d�..�ÉS�hd�2-)�a~\[ێ�%(��G\T �b)����$�o%<O�{zr���k,G�_ss���X�蘇G��=\�܂*�l���ܙQl������.L\�"mJ������Nz�l�h���j����|D����w.8�U��z��%���\�,�]d���<�BY���]�2 ���(yf;Ν��� �� ����/.�����lx�8~������}�|�����s��L���'�_��8{u�u�yy+,�s����k.;�+ ����M�/�浼q��\�y�]��c��2cIM
��s+q�x	�X�0��.�'����N�k�uܕf�P��ff0߅��8�_�Y)��a�Q[�qHx��(��j뛑_��al6o7#�3����mX}a
�����/,�2��}9�����WT���z��W�H\�U��զ%u:��Cȥt�`q"���b8��+���IQ_߂�H�̯�G{��;��ceu�B��8�\��}�b,����f�S"�~�����kg�����HowQ��gq�^�8Z-�������wvp�@T��t�~q��
�_1夸@��x���{"*ܟ�E ��1$������VӤ-�����B1d&�������D��߷�R�������pN4>�z����)��M
�؛D~'��Χ*��+�F~�}�;Y���`F�#��KQ�j��"��_s�4� ��l� V�]qq��w�ť��ˤ'-�jll1z�ƕ
��v��1��%�"I05��:���E�����w&Ea)�y!�]`�)��k�)����!�ΕA'��ɉ��]4�gsJ9!��X�ĉs$��V�Xmϡ"rc(VP,_�GK���,�s^q�8C��)W���^���S������͢��}��5>��"�ĳ^�6����9�����Zlߛ��AP`ׂ�:��-{NC&9�ȇ�P�x>�||�xa7*�0gz�1�t��ҷ�!6r�}��s"!W����T����ۿ�3�J7 ��ȅY�+�N�qR	��b&B��+V����8ĉQo&G2�L S�j_������r�Hn3a�AN�����{;v�I���mIY=�	��i����Pv�R�I�Y�� e�T"�F�\�c5]��՗���u_%�~���(����6d���
KkPB�K��u��5�\����e��ґϼ��*�E�#ى?QT�N�"��R��I�7��p�y2�f�S��3r̸RC�z�۰E���/�߆���Ik:s�������GY�Fn[8+��O�kEu#��f���b��2��6c���@��o�������x��C���Fl�t�sawa�,��
e<vNjd��t�⽬�S�D��[��!]{��w�P����-��L�዗�(��(#+�Ckҝ�0�������>}��N]�ŉ�ST��`��6�֍�-}���Q���6�z�2���"j��v4q�{�N��>�%��U�l+)��حE �xt���uܾ��8v�)�$�=��t%��	�k�Ѷ;����i���荜0�&�|j:�[g�]�ۼ�(���@;F�e7H�\��0�\�B]ȉPLC��S"�7I�j.�YH���˵4Z/�ũ���Ιk��,h�h�C}����9�6���X���S���NauݧΟb�A7(G�-�g_�4Vsqi5v�+@"_�j���\���:ݓiА �ASC�k��<��7�8A{��۩W�i�7Q�* qW]�f	Qt�T9����%'���`�&A�Z<����4d���L8i�d�e�3L��̐�Ӭӝ��`�ZVV���"�>�[�4�~���b:t��2����__�7?��|���ћ�^71�D�L�,=+ns��C��'K�ƫ�=%�%6��Z��5�ҟ��Љ2�f��#9�k��6Jq�"R�@���/[Bt ��E&��ɑB�Yb]Ɩc�l�T��#�eP��1Z6���&���L�����;���^�x��E��<�y��3.�>������D����:����)�i�#Hp�7a����Ҩ�R���qa�OjE:����`��0aAs �SpDǴ�7�c��zZ��(��n6:N�y���C��s$救j6�����US�d�#?\M��^KcP?>�*����;^﫺F/����[�U+Ɲ�䎔r��W��j|�&�)@�_S�U0-ҁ4�Ў��Hd}�$	|)i����4��&�&�X�^wi�V8��S���J�+�_Cwc�R=�Ȯ�Ĺ&Ojh��d:��u���F���ϔ��Ἡԓ��X��.��=��=��i	O�kQf�ߝ\ы�Օ��w��m"Z�XA�+��+W�aB���}��lc��J���j(�?�d�Tq2�:����.�h0�S����Ϭ�\0�kOw>o����M4����|��^��]`�Br�j����D��������2�$lt�51�u�D�������y�q�/����X���c��ۑ4
N�.Ǜ7��ڕ)��Y�u�ҿA�~�C�B�f��i�43F�
���}te�cZB��*���	>c��wr�
�|t���F�+|����P��c�A�`]{�L��!-��>B�!�<,��k��q��������wsr�����e����>��hݫr���b����(Քꊼ��zj�ǟQ���꠻Ӿ�V�y������Ub��_����3��!k�%�&e��TwJ�+�ԝ�I��M����zi/C���
HT1p W��Q�'_܃��"F��R�a�,.<3�\[�c6g���C�R����ſ߷�d��[��K`����f���M�O�d_r�2Q��Jiuw�k�ӌ?3��ɨ0�;[��vd�f �&��������|��O{��������{ｷ�
]#��eߐ?�W�:t��/��S+�&�i�g8RrNQ(����M.�-��cx�
�8e.~�ۊ$3p������a��`*��8�>�/�6\{y������:TQ�X��G���������۫�걈��x��J9.`ZoZb��iɡ(cŉb�jGi�`4k5�Mr��R*5���s���"Ǹ���3��Qv4ZFF�m�Z��/�����l��V�б_��J���:��L:��Y����=j�i9��\,� ^�
����L�R>�Kn��c�r�)�}̤�x޺'��2�h�fY��l���8��6���!�7�a5�)�>Tڣ�?�*8���R�S��ɗ�h���j��ߐɉ� K��z��������/S�u�tgX�.�tvS��xN9�.o9��2�Wڐ���E��c�yÎ���媨ؾ/Ϥ -0eD)���{�Qn�6�V��Xv9�{bEj�)�Qu�B�J$�ck��8�}i9�cI�qpX������l\rh0�`nؗ��1��]��U�pSe�-]�������Y�zİ���n //�>��u�Ӿ3B�YfĘ
q��}i�ɉA���l��.6�6Һ���*�[��1�m�3m��3Y�4;5����(�9�������}G���dܰn6n�r6.]>�X��V�,+~Ӯ���CF�G��
��~Vў�u#g�s�B��Ȗ'*�U����SO�W�;^� ��_�urK��y#�- �x���7��j������^+%��r���FH�,�]:/�\R��|C\Y����S�kU0P̜sul,����2�֍�O1�`�J*0P�V%���2��jLE�p ��a��oА�y���t�!��*X�}|D�/�{�a�|�j��P��x�YM�Ƿ�z���i�n�� dl�ݻ[�n5Fs�m���sq��U]���a*vk5�-�̉g^7�ā��V`�<��{n'�@���P3?��\]$UQ��{˾�ܹ����[T�,��K���T�Z�,��ճ:�,��Ru$W��i�lW�GJ�1ݩA�qM����;Vx*'�Jt�_�|y�?����3


�ڐ��Z�d����|��n�?�������`Oǣ��-��q�
�T�SYӀR�|epu�W�.�ӟ�2���F��S�Br��w�t�6�5KDʅ�qNݪ�£�L!�fJ ��R�}*r�=�$�п�&���s���oV�C ��j��T��nLB�\jٹ,<�0�D7l01p�� �+--5/�ڿ�Y������@�l����d��U�p��CF�O>�$������I�; �]`�3*�T]]�'�xbTKj���n
�����)аB�?�\����n�����̧߃&u
(ڨĐ�����%��ߠ��2��O~�233��?��[`��H�D��]�����g��Q��ʛ�ӟ�8EO��q(�X%=�?�<�3�Ǳ���Q�����7����ư:ӥ�:R�3''gRT���N�M��T�֐�eX9^~� �d�L����;��v>$��q���1��ߕ��f����τ1��|r��R@4��*����	`u,�.kZ�^��ds>�{�=<��C��L���H��:Q�֑#G��d2 �HU'l+x��w�E	��@۰ �E$*�3~��X��2��N7
腓?�я��(�Z�`�c���~#>�'&��Q@j��?�16l�`��٬�W6��P\,�N�A��I�{�yh+/�y�駇��:" �#d�c���Փm������ܨ>q�`E�u�Xj���O�lC��j��]�6X��x�X�«��·��-<�裓��H�l+-{�=����78�n:��唝GaC�����T��H7/^�� ?��>Q�hM��w�i�����p$R��dΫ�?�l]L�,p~�a|��߅�J&[��j�o|���?�a��Pun�+8����h6ɢ޷o OII1�$�~���S@�e��;��G���5���L�祗^2�=Փ&��r�~�_�K_��I�&B5P:���AhVi�Z!�ٚ��m^~��o�<���$}ky����=<��sf%�H-����������V7'q���/Θ��5��wEE�Y�y���wMŘ���`J�������E�aa]�R�o�}����x�G�+���Ȉ�ZO�T=bW�ё09�III����.����q�������&���Laa�)�i`-*�_Ǻ�1��ĶtPrr2֯_o��(�O���j�Tڤ��z�^����%�"�b�V�Z���R�+VL���Y k�*G�zeƉc�Fʯ�������q���={6֮]���;K�,q�#��=�m�;w.�S	zծ9K�7Ξ �4����_�]��#$T�+,==݄<���
,X`�.=�.ŭ|�^{�5��=�Y�[��q��:�ǅ>�=I��ZD�3�5k��W��e��~����RxZi��T�%+~���(�:Z��?����	�u3�����_�آE��|�r�����ͻ��EM��4�����Q]��͛7^�ZMm1l]���	p_7$����h[�\F���			l�K���d��[Ǌr�$ju��ťT�6++��R�]��:O��	О4�����}�H׶>j�$`kr��X�Y���3�=�'�����9���-p���_��9~[@N$C���0���6w[    IEND�B`�
```

### `src/shared/assets/logos/oil.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>190</exif:PixelXDimension>
         <exif:PixelYDimension>190</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
�I�  �IDATx�]itTǕ��H�ւ�7��c��`���,���8�&^&q�2`�L<�c��s���L��}��đY=��9��a� ,b5�a �]�}�e��ZO4�$Z�^�k�
������Uݯ�[�n�r�wt�C�_�`�'?��9p-�U!��b�U����Ȍ3d���ҩS����S�����#7n����ZN�>-����p&�_�T��$HL~
R � ��B���R�{��JR\%"�Riݺ��7N&N�(<���hѢ�^�#Z�r劬_�^�{�=9���3I*)R��H�j �3�_n**�y]��u������dɒ%2|�pq:�Ѫ���z(p��uy������@n�ߕ&�>���Np�M<C|x��d�Tw�$I�$%%ɏ~�c�;w��jժ��ٗcE�����b�rٙ�-��T��R��U�\��]��q�H
����#o��K���5n�?-D��;�ȑ#�Է���S'$91�>9AA$�C p�e5�pOq�*p����2}�tU�.J]HOO�	&�ɓ����#��$Rtdg`�dr�R���o���Ґ��оf=
�+?~<����7ś(R�O�-\��%�U$		nY�`�<���֫�]�)�Q͊+d��q�p6-�40��+�,��̙��g�i0#�G�R� /[�L2�dHB�D�A�i���n����W�{1�u�a��!
���K�}�Y}E�G@��
�WUɋ/�h[��\�6k�,ٽ{�8=�J	�ʔ!�����h�$�=�Jt��?�mzl���b����vK�t��Y��j~�O��U�{�"4lة�Q����dРA`ew\�����2A�z�����KqI�TTTHYi���8?� /L��\S�R%ٛ�1B͈��p��%7'���g�z�3g�HaA���tم����
P��TT�KII��KQfW|��U�� �.�ܒ �#95-MRRS$%9�d7<m۶�;HZj*�i�$M:b�;##ò��]�ܩ�L�6mnw_T~`(����k�RX@�M�e�W_I�WTT*� ��L 9� ��h��su�����н���g\�u��n�&{�k׮��=.�B�HE#h+ih ���n��V 8w_�lܰA.]�(�bf�*8��\	�$�XƧf1��x�����U\��޽�=
��}�ʄI�,�݊�9�t`�ؾCv�en��8q�����.3����F��o��;��5����gϪ	uN�l�g����/�F��3X�wh_W�1��_U�O>�j!�[�|)Y�P>��2���"ᄍj��2��]�'����:�y���)]�u�Y!E]u޻g����Ⱦ�{��DP������@W@��)�ȑ#Ҧmp�hy6=z��9�Q�D܆�Y/�W��Ç)�DK��IemH�B(�7nܐ3���7G���L&O��33��P�Q�������~'��+��&oc��[8W�ݽG�;��^��"i���� �ڹSV���|M�W勛�E8p�=W\�r`P"�o8�_��1qZ4�K�.��?fɶm۠!�+����w?u�6a�ǉ�y��Qצ ���%��巿y[&��H�����3�KJ��{Q������=�OVx�t�����H��fpZ��i�x�ǲf�j	�[ጛL��
0�G0�%���b��2��0��fKZ�~�n�Zx>�o�^�?j���-��Rk����E*2~���7D#�G�a�F�~>CcE�ir��ɂn���&awl�.���0�S�b Cה�=zJf�ޒ�2]Ҡ�P���8�E����R�/��!��7T����X������M��gt'�[Q��xO������C�+�&?m ���-�B�Ӕ6"&���8 F�nҩs'�Q�{�U�"�h����Ζ�}�T�B;���'0�X2Cl݉��)B�Z�t�Z����i8//O-g$�Q����3_�wG�%O?��܏�ɜ���dt-1>e�TuP�\�ϗsg���ի�DBQQ1��N�P�3l�4�:xPIJ 3��ɽkV��������Z���3o�<�ݺw�O�Ӧ<�t�"#��룬,��Fw"���p�[A˖-ugK~ZdP1Dڶ�[�QQRc���-M�Bp�t�"?[�P���|s��U>N�?:��e�Ke����D	w��-[T��z�_� L���24��x�$��X�����NC��W��o��5��:�R�ʱ�w��ȳ�<� \
_(z_��ȹ�)�2k��������%�z/��>2u���Y&��43>�����yJa`�ԕX�N;��O<��_ol�M�&��Zl��4��X��d�	DΎp�+��W�k��rj,�����)�U���fW�� S�����['��\i�����M��Ѱ�5`�ٗ�L��8V����VHlh:'NXǲ�2eQ3�~M������ZU��YD$R:�Pf�Πd����n.#!ٷ���I�U+��*��s��P(�̡�Ek2��ѱCGSZ��?��+��1:�D�L]�R/)�#.��롚0�2C�3e�x��U���� T`/_����61�U��0�ifj2�,\O؉9ãs�NB^�"3z'�604��;1���g��0ש��]+- sV���jVv���F]�M�����r���6�Y�7S,oڰQ�A��P��7�{YN- '&&(��͵{zH@Υ���%r��9=�6"�J��?��͟���F<��d
�����k��*i��ܦNqJ1M�o��19~�Ӣ�����7��OY��k����
�2����B%�̤e>��r�v��mr�
:'��!SΎl��/��ɓd��ҷ__�t��_w��c��U�<�w�n.#�����L}�F��0�s�t��U���cP.�Cߖ/�����*�]�v��g#��|��#I��q����o\�'�Y9v�\[��q�;5��{u�˲Q�q�?ᙵ��I�i�LK'6�Z�M�0>|X���~'���P-��aR�n<C�wt��Ԅg)�P߫�r`�~�EXy���U���ϑ�.�RHeZ���2V� �8J�}��`e<�iVa���]�p8!�x���l	�H���]q3g{�����A=��~V@L�|�6Z0Oց������ ���y����� ff;�������*Άn.�;�T�.�a���:���Y�^\����2���XGܶ���^֢EE�e��G�˘1c�&�ol�g�'�c�"x<�y��ap�Ye�]���O?}�۴��`�����j0&��bks����YF͜�~.����`�3B#��k`�\u��Lp����/���~��j7S f?GG�_��W�1� IZ�Ե+a��b\J3t��̙�*�u�A��U���3e��T �uk��8摫�{"�<(��u�"��x$���Ʀ�tB���+������r�Fέ�Z�a��HFwD�4d��D`�I��/V�t�Y1.���S2���d+��W~�RN�"ű���Ɗ :�KPɭ�m�`��O>)c0�u��� l��͛?_�a{<�Tb�ym�I�x��(�
����aX	A#Ƙ�cժG�ޱ��*�FE�����r����[�N��)���_��;BY���i+n��zp��"���k�c�G��G�2�`T�2�1	e^{r ��:uJ`��.��َ����`i���#�����������:KX;F�a���[Z�l�����*�1�������
�����ЄE�pwͫW�"��aۂ��/�1������=�7 T��V^�J� ��I��H��}O��c���8��3V�Q��/���%%QõN��R�
���9ʻ����1"�\���yYy�E#�8?��7_��F�G#'%8�H���4���������dʴ��d�:�6}b�f��Y���#'�vʱ万�+ )�} �3D��
F��'�kR 3?r%#�S��.��;N�ycn��Լ�"'��6�8��Уv����� ��	�� �A�F�����!A�q!����kx��BԖ�����!�e�v���)�6mAl8��a�;��WFgqbC�HN "=p�;�L�`i6��'5v����I,��mݻp�����@��fai�ɹ���/��Tp>�+��	}�Ã��=�⺧�����8��:��?�����#�l%AD$ `���I�8+�,K�ȸ���5G�t��p^t���6k�J$��
05&@$>�-XX�4����R���н�F��M��Vk�кU~l,F����a�k�w���}�&I[ 5$��)by�.��D��m-��k�{"�e�JN�'��"�n���|���8.S���� ��&J��8���!d�UD[`F���7�9(J�Z�k�p�Ŷ��6��R*N��S�"-�p$�j�0-���x�p��WY`+X�X��>��: ����N�\G��K�S�8rZ���8��?+��l*�q� `+���p �ĵB�m�����e�6��V��� Ǳ��v��j���6ȷ�T��[���l�;�������6���x�� Gp�Nj� Gp�rp$�"�A��-n9�a��N͓Nƶ���q͓ı�����)^墅����q�b�;5O
����ѣ2b����1�Xy,.Ah���EbT �QrXU�݆&�t�}���ܹsc�9�*e�/$�=������݄ ��տO�YE�ם��{������-��h|��c_�8.��t����e���q\���Q�YPRU�nݪ����F�z�Q�]���琪��^O����W��uQ��_y�qA)�tJiY�����/���Z|Q ++K<�xq��T��Rpɉ��w޹kwH�/�/-co�����
..B�7�+Q�����������hh���*�~���	@�����⒊І�/���l��v�/
p���˗�޽��/	�_WBr�e�� Ud'��*��/��nݻK�>}⫖wii9#��3m޴I܉^�^�C`��X����� y۶m��=nb�[;Y��m��~����G�a�������:��`po��EU0�Sg�R��=y	>9�Q��d-
����ҥK������U��",�#�M&,4�V;��� {��҂q�~�� Y�p��9Rm�j�j�}��F�Ԕ?���_�ߑ 7J��"d�2������ �ŶT �R c�P�Krc+�Ww�Y�%�&M�ٳg��ѣ�>�Z���fgg�&���n"�>��"�s�G^5�t$�%#�kpr��ЏIO��u`W�r����'�d��ǭS�d�E���Ȇd�����G�[L%�A�eA�,��E��j���?��\��    IEND�B`�
```

### `src/shared/assets/logos/ongc.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>190</exif:PixelXDimension>
         <exif:PixelYDimension>190</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
�I�  !�IDATx�]|TU�?�LzBz/	-RT:�,��bw���/
���~�]D�"��ػ����g[�)�H�J@:$@zB�����'1ɴ7�As~�̛���{�=��s��Jl_&n�Ji�&�i���2����I�V�$""B�{���B�	������P����ȑ#RUU%���R^)R\R)�RQY<׍#� �B
��$�^���gam.Qv�*���$�߿����Oz�쩟��}'99i����+a���eǎ�v�ZY�b�dddHXU��FEȱ�*�+,��
�~$e��^J��J	�N�	{���K�cT�^�!1"v{�t��I(�]v�t��]����iG�^` //O֭['����,Y�ҳTJ+m�_X.�e`m�Gڙ�T�>#�2�	b"�%���MNJ�	'ʹ瞫��x��Ca�ﾓ�^zI/^�*�I^1�vQ�QIϰh���J�n�F\�]�&D���d��Qr뭷��'��L�x���8=w�\y���СC�mr$���Y=rs5��.͚`@//�	&�Dpnll,_i���ҥK宻����Jy�]疀11��V��d���18���\n��v�����f��`���*_7�|�l۶M�l r�,^���4�k�N��\	����������T�9s�t��U¥B�%FI�������-<cn�T��]tQ#�>MQô�4�={�t����Ji!���	�9��c)P�Fʴi�$::�4�}4k�L:v�(,�r0�K�*16\���ʤI�$!!!�[�X�:10j�(��%%%b�8>���T���:�۷o�/6>8~0@�k͚5b��M9<�
�.2f̘��5�-[����ǋ->�*p�رc�M�6���������ѣ�����x7n��U���z�{4A&%%y�k�*̗�>l��\4^[���cc$�I�󵗖�*����-�:,�6R�h2i��a���Pz\s�~|��e��!��������$n�=��%3�Z�h�#pQ�Fz��4� �i�_���8���쉉�B��x6��dX=Qh[�����Ni^�TbnH�EEJdD��޼��4+��A|��7<&���o�*Q���ٚ�Xq�D���wl/1pj=�%v|��v��,�����˯��2X��zFl��dp@ѡLٿp��c2W���={�p��3���`��C���:������]ƁzNN��b�!�e�y�5�ܻ�~��ڈz�p6���;	$lL�Vժմ�$���{���*�zk��- �~����ˡ�KY~��fpv���j����NG9~B@.�`�K�;uT.m1`�ts���jBqf�m�.s\�L��&t�(a�@P����_r,+K9������:.Ƕj)����i�B�H��")ص[�_y]|���/�E�p44��o{m�F(U�K�K�*��;G:�=Z�z��V�cG�������K Bm �)���N�47OR/��c�n�m����3�q��9\)�����6��l���k�b^	]:�������!9�j5��3ٍOEI):����j�Џ��t�
�5[�arY�O�vqm.S�
��f�\%��/�g���DS�"���ePx�6M�޷N��ϿhT�`�K�x��nۮ��L(��KQ����U�1\Z���$���9K;�~��4{��n�T���3�JC�6�0[w����%��\,�ݺ��WѸ�B����R�{7�#ʙ$���Δ����E$�g�=�f�)��ӏ0���F��XWB�&���Od�'��{�t8�Ҭoi�*!ڎ���4P,�F6���X����D�3�����V5T\�5�;���͸��)��M/�,{>�\r��-��،�<�>��s~��mt|uM�q�f�p}�N@�����r�G���Dis�pt�����g�y,?ZZ$��|G6�yE��Y�6��Z�A�2�,-�/��G�]1NE#�OD���7${�f�4r�+�Շ'�ቩ)լ�*ifZ���'J��g���̃QD�@l����]��_͗��+i7� M���h��>�n���I���a��S�ut���Գ� 0����0^ڞ6J�Jݷ`��{�9�f�NM�J���8^�{4bD&'z-N)9�;Q���~ �}'i7^/=0e����#�����>(�>��)m���eI�DC_���c���0�I\��>,�o/G֭ǜK�.c���W#=�;�~��
Ĭ	�%5sJ�`������!Y��#���{�k�O4\g�Ϟ%�{D����u	~��`f�#����2�՗0F���dB��E�$��WT��e=�l�0Ib̬�#��)B)	8�o}��v�Dٌ��I�r�9eƓ��5i�}��hsn;h�#g�)�i9J�Mي��F�)Q]�qw�b>1����u��t�"�
������LY��Crt�F�أ�Qw� m��X������:D����yQ[�V�Sf>�$.��5Ӧ��)���
�j��T�E�fm��S�~'ILsxm(7�#d�)As6�k��/<OO{Z\P99�G����>➫/��b�S$��7UA!��������&{�B4�ֽ�e����~(���q�ȠiS�Xա4�z���e�^�\<VR.�U���ɖ7�T���Ӊ��
9�O7��|�b���i�/�VݹC<�Hс��w�B����PN����CH�!hf��e�ׄ[�M���?*[�|s���Y&�J��0�I�;���y��oѪ��9�:M�����!2��'$�mk]r[~�=jڳ�C��S����4dФ/w�Գ;̡�&6I�|غ����c	��	��g��u�����<��R�5ڮ�����cA�{H��j�!�rm�k��S�&)���,���=_|���X�
�{<<�c��]-}{PS�PD�b���Ƅ,���1�k~�I[!<5g2;�� I\��6o}�]��?��m�5�����2B  �F�[�+0�<��Kn�C�w�rښ�(v�HX�t��ai7]o�=L�S2j� �iO@��A�h1���6Eͩ�X��ɽ�m�J�)J����-�{���'y��L�;O!
����7���� �}�E�����@|��9gK��N�x0SK�v��ҤKg�����w�8Y;WBJ�jˍ�_��s�k�4�8Щ`��(Q���Ϸ�Vsq@\�Ppz\w�����҉�jCFm�Pw�w��דkK�z��� A��` �Hw�1ڎ��a�3@`!-Uto�x��������	e{
\��TI��x���k�c�Jrrt�S%���"q�譴rYN`�Ƿo�+6���A�������D��q��ΰNy;�n�q��ZL�ZO�dYY+Vʡe�k�����B;������L�F�����:�&��ܛ��|x0�P�v(>����Y�` �)բ7�1�0�!"��BZTK	̞�]i�o�Jr���?2��9��4���ꚹ|��<h>�����urd�&u�����`�=BM�N��m~w�����R���Mg9:���9�E뭞4��YR��pt�$��_�w�[@�3�,�tX�.��;�i0-^��[5[J`�6�06�����OJ1�Z�I`�4h1���u�鶿7WqO��`�c�?V1Ͳ[
��N�n?+c����9�����ë�u��B���:��#�'���`�M��?UDk�(��o<Z��[KӾ�-��YC`���f�?�t�^����ϗ�l�
�xf���k�d��{��5��[0wF&'��{�ܾea��n>$4q��gZ�;k�hg4|��R3����Biy	X����^�vׂ�A%�9�}!��V���c�k�@^S�"������W�G��CKL�9��Άh��>�g�53/�QAj9x��b�>!O�aֳ��\n, ��Ρ��f�Iɵ�؟�����-㰧m�5Sܴ��̜މ��'p�4]UQz'x�L#M���[=�S����b�3F�C��U�Q>�F%G��k�k9dHh�b�	�w�b%�s��t,h�pr/�>��@WN4�_[�؆�}�T9��>&+���W�=){��JJsr����-�`*܈E�y;A$f�#�P8��j*x��������������b��X��'z�
��:�no�GJ}�]�,!0����*�͕����Ġ��ؑ��NVS�f���r�J����H�u�,Z$�	�����nM��I�L:��&Bh9r��aՂ��Y$$G"�a7隢�ه=�ܱ�
�k}��jr��j���^��%	���?���O\�M`�Frӡ��6{��I�SS<�ˇ�_��M�������쳌a�O�+��A����/һ��?�Y �`j�[�x[��5�ܸ��e�{^�3/o	��T�F�@�����$t�k��
L�|k�R:7���.�-�d��E.�1Ҏ/D6ߡٕ�X���S�ڵ��di9�M�Wi����\"�M��%QpL�7�֯(l9��pCtV�������7ז؛­J˝mN�Df��\�x5�̧ە��NȆլ��� 0��ӹx�$�����eB��3�w������Ԝi��e>	���s_-Qa.q�����?i���r�'B0@ 0���.:�QM�5�NX[��3�ֈ�l/g�q�kG5��Z�ho�ئ�O�&�>tS��n��^{�sВ��X�7�D���h�s�3͖ܰX�\p��:�b�<4�֧���UL����l
bq.n���3O�c�����[� W#�'�X���w;vb���/y�O` �4�@�4��B$�R��ީ�{�	��/����UH;� ���N�������`��T���N~��6ƭr�ۥ�jM�6{�t��1N��&*s�nsztp�ni�#_g��\�c鴘z��B;gC�uf�L�r'!w��D}.\f%�"6>6�iplc0 <*��]��]�>��%�+|,�of�ܸ�����m�>����w���m!�im2�Q{�.F�����I`%��"F�bOk�{:2	6�DD8��������9��RQT�#��	4PJ0�/�+	������&��O��n�
s�:��]��.rf���S�ⷿ�Z�!�2X��6�׮��s6�xܴͧ�t����U�%S�ϝt�3�i��s�6�[��1�����	�J4�=��Y���U`�0g�V%0Wu=6k%8"; @��v��
g���/���0����)�\LH��C�GJ�Q#Y����2���XB`V�
�.��׺	}n�q��0�c*��y�ɐ�'e]�R*0r;cI���OՕ< �-�N
��ӎ�z�_�0��Ph�� �Oz��`0��B��T�#p�-��[6t���g[�5��4{s���VT̗<x����e�r�������şwJ�s��w��'/�D4+�qk�����]*G���(��&&j\׻��|��%��(�K&�&�8o��J��:Ҽ��eKXg�	�#n�V����h���(��O�0V(YD����A3p�L(�\�<�/ߖ�`��Mb�ơ�3>T���(?&��$����_|��0���E�P�������Zn�,�˳���7E�S��\VK�y�ִ�K/v[D2湝�=ۘ�#8*;I\�6Т'�X� x����]d�Se�Qo���ST!0K� ��'����/�6���N�!��B��m_|���ke�����a�BFr?���WkŞ6�!�B<zӎ�����ZީB`6�DލJ��i�c×7ͭ;-��Tr�=W�O��Kypd��o��xH��a����Ւ����3E�2/xqw8�׶[��b�/�܎Hh��6�_�ؐ؆k%���F��}�-��X��ʲR=��[n5����U��َ�L�ϖ��x���n	\t�zN��E2cH!!{^���j�E�>��Xq��pt�7� ?���6�@(��#�#��n�Q�a|癆� j|tl�t �z���������m���浙��o�c�W-�9t���w�ݙgȉwLRGz��`e��~<���BN`��Rt���2�����Y3O�;�GP3�[\�M5���9O�N�tܤ�ǇFZ���N�%l�(	�N�Ҥg�����o��Z����b����	b�4�����.��zט��I0���u#�<p��	��n��:vd�`Tt�}D���O���+�\C<�`���?�}���'N����[j����r�����N�BT���0��{�X�u!.	K�%".V#��Y���nK�����4���r`�����͎<9�zh���`ک���.�D���� 49R���Dڝ~�$e�d�e�y��A@�VS����Q�����8�kҞj�̋�T��2揲B󸟣�6(��q\�6����|�������w�.C��=�qI�LD:�YTH�"��T�>5���@�u٤���a�&ѨB%����v�,�:[����2�B";C����u���XOO���_�2�i�c8�D��q��g��3�ЊCBND�`����҈{I_/���*�i�Yw ΅��+γ��S���+!��u}�m�!�&O��K�9��dq��r�`�dN�L���=P?���Ӄ��T�f=�N�lG(@h����_'�?��kD���G���ՄGRk�?�>삿
b����-�_]|�̿�����Qo݌Y��ڛ䇻�W�L](��ܟ4��J���s���#'C�o{���#gV���z�����4?�$��1N�]����	i�1�c.��q�����1�D$9|�Ȋ{'��]��{���q% T�ۋV�E��$?N{��mu�=��;�qQA�u��f��`2_8a�M�'����0C��)��I����a�l�R�Ep�ʤ�U��t�_u�b���Ztݍ��=C��a�u��ET2�R�M7(�y����q(��cT�c���f=��S_RZO-�~�����םC�(ܻ�0Nz_FC��f�����o���;:�Y�`~�W�{�b�n4a˛� yp<s�
U�����z�P&P��v�=pK�D(:9�c�	��7 "������*'���0�g#x�Sm��'y/��f��\cf�fOTs�j<a��ٺU��Qτ��/�F��&)�g������
{qM ����jn+T����[�L�C��8��ֿ�f�� �ðg������Y���/C��c�e��!P�S"p\6�Rि��P�PG�Z=u�d9:;	M����u>m��� 0�3��)C�2��RL�x,�)F����O �.q�jNc��2}n�����<���i��cdW*S�G�
��J��P[�޴Y�<�8,c�ɡɽ���j\�z2 �g���67����_A���]�<$�`Nu����g4�>E�j�b����0�׵O��q��D�N@�M��M����_��ce�A�z."����lza���3��V.O��8�<�!���V���rg��Jd>��.V�	ֆ IԌ��|p�R�[��szrs9�*J�ɡ%˪m4clʤ����&nJ��;u�S�#5�Y*x�WsL�B�	i��։�9��D:z`2^6�{�ܶ��y��檝z�>�I�5���cOe�U6�~[e��la��2�1L݄�t�|7��g&���.�:1��ޯ�;�lw�(��͎�Po�ař��.l��%����7 ��fʽ�\U2;(�L��ݨQ~7����+N w"��z��������*����s��b � �gDa���Mri�X9e�r������"��.g�X3��e9^l3|����04`�O 3
��_!y;w*a]�ǫ!�e'�\R5��2�P*[\�׹��LJ�rf�U����X���9XR�ř�sa�݇
7u��`q��gG�YMt:Jར����SQ�O���	�V C�^jMB7홙��e��ӧ��ՂX�ܐ�ar�w�_v�%U	��e*́�iع,��(�|��[�NƎ뵨���{镘���Ҙ/8ޡy���|��9,55��[�n����K"~5¯6HY�
C��y�~]-kl�b�������W8�b#�z0���-�<�a*/���7*'�z��ؒ��Ol�P�
�	���>��a���,Z�H��I>L
�����`��;w��ߺ�x���dƌB+�[+�-����hy�T\��qt�6�D�>}��_�^g�k:N��h.((���,�4i�l߾��m�o��ngϞ-���T@"�����`�tu	����Ȑ[n�E�8V�o�P���駟��{N�0�E(�28E���쓙!���ⱰP����˗�	���k����̙3e֬Y�s��Q����� 0��&W|�_TxLbẚ����WJJ�����2�O<!s����$�\H\s�P	�U7��K����^�`�zIi�,\�P�(ѫW/�s�:z��m�h���r��w�_|�?�������'�(=m�(���qSS��Ԯ��&Is|ʰ�ޡCU�ƌ��uF�6�*� �1g;��R�R��?�	K�#����h�?°j�    IEND�B`�
```

### `src/shared/assets/logos/reliance.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>190</exif:PixelXDimension>
         <exif:PixelYDimension>190</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
�I�  $�IDATx�}ip]Ǖ����}���E�ZH��o�f[5�8��Qd'3S.���q4�r��ϕ�\5N)�H5�SS��g$�3�(y4�(R)�"	�I����/������ ���h���׷o����9}���_Uc��p`��N����P]��	��q$	���`��p:�p�\(--+��{w�@����B�������ǃh<��H�X�Tw�M�݂V��}�2S[�u���"�) �@��F�ٳ�w�ƺu�v�Z���Cyyy����D)�������|�2>��#�8q���p���R������Š�R���J��^fb����y<NT�y�q�ə%r8�'�x����cMn��J�d2���~���x饗p��i2��(��@ǓdP�Y���A���@��qlE�ir�u�����c߾}�jr����g?�.]�H��&��6 ��ls��}�)��f0�*���O�M���O���&��J��E��������+� N](I�/���-0].�V�kݳ�������>�,*++s�c>n) L<�=꣝H�/v"c�k��_UCZ��N��2�ߖ�?�)>l'�?�`
9r���w��=�~��?J@Y`b����^P�K�n*Tn����'?�	y�EχY@�U�VaŊx�#p9�YX����|��q����[�c�L}�k_�՚/b64\]���I�K�i�H��f\C�����韚>7�����C����CC�cǎBL+�����)�;�m۷�駟�=��/iN
����A������f��{�ym9'�fW�����������3�������gWM�K;*���k�P�DNَ�z�y���}74�}��G���s�Ν��anQ@Y�:%����ܪ�]_���*<���pܼy3�dɒ�� s� �^tp�03�0�8_'�ḟ�L�y��2���{�׏VY�@셬�H�$�J�B��a� ��G	�#2p��"1�z�t �G4Cҙ@"@����e~��*�����b	�����U�_^��Z�s�Y�dE#}hm9���m��X?M�N�ȩn��	x�rl��ő���>�0�x��6��Y��)�~o)j˖�]� ��eXܰ	�5���lD|��LD��y�]�=n\}�H�	��,!H^�D��)��.y9��o���)5��Z��؎3^�WZ��4�C	6'�"M��.چ��C��(�\4�p�5 ���r�#�6��H�U$cA8��J<$�� T��r)}{9��.�濴��`�QGN	"�������Hs��M)rvҕ��ڢH���u���r�,\���c_>�Ì8���3o���
��G��K�)v���D���Yd&R�An��O���;��VbY�á�t�K����yGLw���#G�h,�����b��Q�l����0c�D�h�x��������u����c^�C`n�i��]���`c�4hJ���4�T�`�jB
�����ۃ�"QVmxkwB͢�J:��Z����o�y���+V��#i�,�HaCw��o%䲃�-TuG��օ��İ��"�t)����J"ۼ��X)��Yq�d}� �e5X��_a��G�/)�p��(��>9�O����(u&�]���Mp���,��)B�����,W!��a)��o�aTؑ �-���R�F	*}q��ʗ��=�,^7Z�)��1 �u��������>���GQ��"�vP��p���V��P_��#7��Lb�M�eܐ6Ϙ��A`�sM��W��
����_
�/��4d0��V�M{���f-/��0# ni>�o�5�I�]n8�I���������� 
6gq�� ��e�-�WZ6*栧�P�4��X�C����M��䗢`�o����T��"�����i�=%����b�N���#8u�x]Q�p5E���&�,�����RZ���ICx.[e���A�}�" $jm��H��l@ҴA�y,��;�&�R��7�3B�N��9��p'�ރ}���~y�V�L+�>z��i:tӂD@iJ4P�PA�J�����^zM��,B��q!�e4^����=��uS)td�g�˼�p>3���|��:�I��*C�s��$i7����އ�3Ter��������7�}g�ye���#n���dy��"���1y\q����]s�ǭۙ�BJ�x�wX���O������I!�U��Uz��U~'t�e�����8�����YL��l�L٫/��5>>�?Q[�����flJ
�4��,�@Y�!pD�`�(�&���$Y�fEf$�͹S���� �_j@�-��ڄos��ϤWt��{y��q�q��A����);�FS>��#�}�W�g���P	;aLʨ4�\�\Z���Y/��Ħ�W���\�rõ�l���9SY�k�2��`K�����׺� ������gcU�B�z
g���1#r�f*�L)�}]�q潗Q��C�'�k32����&�M�*�Φ�lģ2R]�J�|se�T
�)w��ʕҎ�����,����d��|�K4����G��{� ��8��_!�/'�O)X `]I�����1bS0�\#����lpo��<:m\.7�8}��_���SV�)8�aå�!�u�2�ҜL�W�6q�i_":M��ߌN�`�&���f�I K��<�T�3Aw�Î~Vґ�֔��sv����NNf��zE)'I�8Cu���C"�΢h�S�K�q��oP�m�I���e�!7�(2?�hj�e���ms��!sӾ72�i���Z	9�[M�t�uk�P��H��>x�
Q?婆2VhkF�N	;�A3)^{�F]d�#{MI���q���q��8��X��L@/|��J+U5�/��ŉ�Z%��h�*������YU�|5�R=��T�U�?���.��4>>�*}�"E-N�$�yG��Kh>�6�]M�I���93�Js��9[I�H)��v_�����Y�+G��"���]Q�\4�����p&\T���Ƭ���DI�͙I��E�������q�+���� R<��h ��6���cn����Ԑi��Aߩ~xP4��&�w�؞	by�M��/C��כώ�Ѽ���3Gh��v�ۚ��|�-��Ԅ�i;�Ӗ�yy�$������5��1�pw��4G�_�ʸo��rqk����X5QͽԨ�gq��ѱ�N�^Q ni:?b�f��oE�'q}�Y�Ҕn^΃_���q�-4)
N�(gL�?;C�A(Y���C�Թ�;�aWG3z:��N4�؂����{�r揊g��4�Z���n�H�پ���\S��Li�F�4jw��mFO4�;���c��n�Ȭ��â.M�����E\ L�06mJ	9ԉX�Rl\�D��������T4�ϥ.}� E���&��$`�[�xY��OϚ2M˜���,XH��r��hm�"��= ߧ�]��e23%�%��KȘ�)������-�OT�1�'�A��AD	�Ý�U)ʕ�Nx��V:��w�O���-��nQ��v�����噢� �w!q���bH\م��(�W%KVA9_<��j,����=������kDY�Rnu�2P-5���"�[-�q㓣�QʍX����4_&���ik�� ����g��5u$;Io ��)F�%�Lǟ|K4N�K���l�FniG���0Vo����Yo�1�=��(��E��X��\9��O�%���1���{��rTWR�@˞�Ơ��KX��������iE����r�!�.�0A��J ���,:�|Y?��q�>\u+>�-{��k��~vxBʪ���?B�����{�����fN
8�b�[���n]��6�B�
�4ڮ������A_cO�_j!�ў��y2�yR��\.��`�ާ�������I�`�:�y�YT5�Ϗ`��܎ך�V3-nH�?��5ΉPu��` �RIn���&`��s	���I蓙�#q��y"\��1�E�U�������`��Z(��ý_|eu��Mܬ�@.6�B�I�p�C�?u�B� �kF�['����x6E��$WR'�fGh
PI牼T�_,y�y�cσ�E�ꍅ��m�xhx��'��d@��Rz9�Ko(Z 8H/�����U�0Z�|���R�~�EP�����hŹz�\\'J5�8�䤣��3,]�9g
YS�u�w J;��7(Hjc���@�^W0��� �=J땠��1*���:�̺d�%��
��A�v�?������Mc%/�=mϰr�~na9�I�6�μ��d*ث��C��Y<U�����h�	i4d�L�?��:��C��e�1t��+5q~�/���p��=�'*bL���t5Z�X\��k�q�,|�l@8��%�[A�������g��,*\��]�*�bۃO��.�����pz��QdʬY��fc<\���МV0��<���P���y�:�O.���J��?�<Q��{fIO�9��9X)�+;���}/�k�5Y)����S)T(�.����0�&䖓-���_l-��F�(]��6�����T9�j��,Ս�	�z�^g��7���9	���0��2`-�t��5�\�Kc��Č��}��#i,��M�ӳ�Ve�6O�v�6�@�o	nk*��
\튘���p��T����Z��S(�dZ�0��M��\~N,[�gRd���ip����L�U���~�"��j-ʁS�.�*T�P0�=��j=�UIٟ-�L��l���y/��
c��P9�P��Lp�D�w�@ ��`C����d��6o����+�v�_3᳿t!��J%,m�4�	���E��Wl�;���()�q�����8�O��"��3!m���aνH���|x���GJ����Y�	q���W`��bi�rTT,��%j�q�}my��'aO�%���"$��S�\�I��iJ��
9K�O���д+��0�=���@�]ܿ�[��3=��J{VX"{XU���v�T�ײ%�;�F{�q���C���V��|�-��c�nJ8+fm�'KJ��Y�l`�_ס�h�,��:��::*nHh7ǡ7M�ʁ���W/�͑�������gʧ��r���a�X9�b��!�ݭc%׽.�A�q��G���TE�!�܏x��J��osD.M�j�6x�5��L(�� ��+ͪ9����d�'��2����eR*Νwʫ�~���}�W��~� �d����W8
q�.k�?ŉ�� �mX���YJ��j8J��(>�u\��_Ar4/*(�/�JB��I�]k�J�&��-���[񍦏�t���_���vE�p�đ���v�R�l+�%�j3T���J��,C� f� ��:܉�X;�y�� .S�Zv����/�J�ܴ��.���ŹYwו�%���"��2�\{�y����׸��,V5�4L+�5a�Æ�2�&rb&�d�e��a"9��LAv҃��a?��V΁��Y����7���:����~#n���{�S${�������^>��fYy�H>jT�����_�3n>^R^F�_��N����۶�� e�8t0F���Է侑��D,C���(�̯a����؂:쬫�lF�7X�85P���λ�8L���
���k~o�����AQ��9q�K@:n^ƒe[�"�y���z�14��'8B��,����C��ų����B�����F�dC�8��cV*�f�^�d��"DM�q��Mxl	�N9��vsG �p-��9.#�mn�z��o�H�$Ǫ*h�/�G��`	(���C�׫]�������\�z
m����~��O{7�wJ�[q��	Z�	���w~�Q��G�@N��n�����]�Pp�KJ�P�p5����CQ��wЮ�fo@k�[F{���D{o���۴��rL�K����kg?n}t3�߃������@�)���(g����	_e%����hU*#reaװO�P�(lP���KH���R����#w>��t�KG*�e&�2aE#�����T���=��@qp�9i��p�69p��t�¥���"F`c]�(�XAǺJR<�
�\��K,k]��t+�pt�+.�Y�B����S���q.=�ni<//
��סl�V�s���ۼ��;?u㳏����]?E���zJ�V�v���#�BJx����Ş��M��:ĘP[��F�b�gҼn�leLBS"hh�=E��LXO�9��5��S��+vL6���`�i�/�Y������Qv��nK-8ۦ=x?s!��k��GU��Ɣ2Z����xv�J�]���{�]�� ���B\�{u�O�r�r��H뗛�L�R��a�T_R3}��
H���ո��(�I����A���*�|j|Ɋ����7"F������؁��7]~��k�캉��QF�ȸ�R˕��9"���p.��6�b���8����Q��"| ��Pm�&(��+�ID+�l�&]B�Û`ݷ�z�$d�#trwW.Ê_��[��Kw炸=~~S�0��i00Vk�x�'�	�'J��P)��{���|��zw��"�����Z�8Iy777�{�)�w��m%���:֐k,oIK���~T�c	rù��'�����!��x�Sf��Ŭ�c���`�dź����8�&A�+�z�ˑ��@΄�`.��-?@;7���R}�|��bS�ËTX��wv�x[>N1v���^��H�`�X6�uR���3�I�e����g][C0�W��9��������3�Р����|��(*��
���*�:�SQ�Qd�i��s/���~����R���C���k,�4IX��A.�I��Ae���*���9������_�Z�Z���e�yi�Z���K�6�y) �уq�� i�ے$�w�`�z�`�_/�;�
��Y�h���CXo)4w$��;����si\��]n��OTy�3����NM�QA�)�eI��t5#�mr���Z.��\��	r?]��և6��hԤ�Ub�l�H�D��h5"�"ݜǮ��A0�B��/�q��\�*h\�Vi7p��b����fj�Z�b���,�P���|dH�D�p?jhw�PY�V�HK���rL!O�∘>bn���ʿz�*<��_��h�Q�� �l}c�e�C�?�[/�9ټQ���<��� �?ֱc��l���6�B_M	��e���78	Q�O��n,�C(Vg��	��P�������pqg�9� �9-E���f�k &����������Ф�r��j.�������A?7lIIV3���l�R���kajޭ�W�й��u�L�k8���s�_� ��,n܂{=�������e��\�*�)Z�����$���v��6���4����s(2�����>�ن�۱��7ї(7���]Zi�v;�d��P
��{
K���T�)XZ��~���Uj�!�Dd_6�{�F��
���P|�GQ�CM�t���4":��Z�8I���y�F�5�wa�^�/@.��&�K+W+d�K���k�����rㆇ��ƽY7�9� ��[�}+6�R���r2Sںp�2��0���ź��+�^Y�R����>P}Zv��g�`%��^o����J�i��h����6)r�֖~eg����?����8��r�Vl��߲a�"6ua��0�[���,'C���D��\FC-Z�~r�(^��X���fj0-1<V ��:��܌�
kVi���=9-�>��m�?M�Xӯ�w�|�C&�^�� �?�<<%�7�;�)�S���/r�>�m4��"��ݝ9���n\���i��i���pe�28��0N�aK�1V>�uS�VV7R�o�F�$����h�\@�P
�������Yf7M�M�=C��NQ�r��z�~�i W����b��;�1�mz=�&h�6�$C1���c��h�zR�����w������� �ב����&-]5�It?WV���S�T�&��f�C�%3i�:�n��߫��	6��`�V}��Jz�LS�6�U_7A�����=����uPdS���"m\}J����7�q��д��[��4�[��>\�YA�r��X�R,hܐug|�5�p���E�Q��db�qe�v�6�Θ�;��}��'ڙ������V�U'�l���]�&���T��`?L�FO�U�t7z�ԭĊ59V֞ �W�p�E��PL�")l��%�L�fUu�Է���/�E�K��<��}��9��=Ї~Nfl������?�ۯl2�ӳ��
���k�=���\2�	�4rĒA�1�p:[.���H����B�\l[��D�-h��٬�I����/|�&͕4�$�x���e/}�ص�]}�5-��{����Wl�z���J�O���ѹ
����_�&�F��U���<�����������q���E�u��͆�$7S~�ʟ��~m�WL8.I'�����<���9�J��ba#/����λX�8�������p��8�z�>��v���w������
~�S[�r_�M�>�ͻ����9#Dt.��.^�{��g���T����{�v:��W��{(���۰x�^Z�Ov������@�νO��
�w�G�%�rd�P�o����=������w�� ؆(�m��D����d'e���~䭻�����Fj����}��3��
�<� s�'7���m>�E
[�ѣG�b���D
[�k��6O�9J��_����78w�����[���������������K�9Z�W_}W�\����B\<�Ͻ͇�A���^�����?g��8w�,~�󟏘W����j!�Yප�W#gw��Sx饗p��黍s��gΜ1Xj��Ͷ8#��`(�/}���?��9ϹZ�%�q�~���\��á��B��(+����455�СCF~�%t��x�������S��C=��������m���hnn�y�A.p����⭷�2>n�]�v�C�4s���H0"Lo�Jn)�t�2�]��ݻw��w֞e����
�����QC~`���H6��dV}������.��VUU��ŋ8y�$��ك���P�Z���R@�����=���\(�Fk['��G萏�����鴖�"�p8������ގ#G��E^[�l�1���5�K/b��_�~��
I �;n�n�lE��S@s-��L'	`E�Z�/^��@�M��m�L��v�2���LN�:�_|���U����\:�3��k����� +�C�    IEND�B`�
```

### `src/shared/assets/logos/sagility.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>150</exif:PixelXDimension>
         <exif:PixelYDimension>150</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
��Q�  -IDATx�]xTU�~S!=!$��^��#M)��X׺�������꯲�Zֵ!(EQ����Dz� R�����=�&�:�L
��3�3��{�����sϵ���k�e)`+���������ס}�kcc~lmm����Ny�VI�`G �������mڴA�֭ѲeK���������� ;;[}������h$''#33Sϲ;�
`c.$�5k�]�vŠA�н{wx{{�iӦpww�����&M�(�9�����yyy
Ԍ����Odd$N�>�S�N��y�0i�ox�!~�i�:�����p:t��ɓ1l�0���*@ɝ�6s���|���*pCBB�m�6=z��� ��:봷o�|�`��%ɍ�;G��q��)�kN M)��=88�~�-N�8�˗/+�N=���XFJ�R�%�68�ɩ�~~~�2e
������ߖ @m�$�gϞ�ɓ'����c�޽J�P%4��` &���SgϞ�9s�K�.prrj(���qqq8~�8֮]�}��)#�\]�y�L#������ܹsq��*ܐ��Rd�.�2����q���pc��mU[��z�Kۮ];�W/^��W��Fv���駟�j�*;vLMK��S� L���JQ����[�nu��:�����]�Ghͯ���)�4�8�	��/���Ƣck�իW�z�j��t��ʏ�3�ɵ�c��~<����-���t���˗/Ǟ={Ȗ��v...�,I(r-�T�=�t�R<��S
hK��P˦�E�o���*rv��%�Zd���d��w�}�n����vQ-M�0A�X/\��"c��b \��{�1�o[�jU�Dl�Qw��	���S���dYD3v̠�s�=�|�է��A�%K�`ǎ�J�rs%�s0�)����o��y��՛�o.�E94>G����T$��3W̬ �J�ܹ3�{�=�c�8�Jb삓)��`l���9@6��}��x뭷�h�U_�����l�C�����, S,�` ������S��4A�.8 ���ZLpiP��_�¨Q�j�3�z
��8p q���2#}��
`\��k�aڴi&Tk�Z����k׮�
�Z��O<�f͚UU{��k@.|���1v�X5�V�"P#�ɹ�u��`KEajҡ[��6��׿�eKT����h��4�(���ɲ`\��
��߯���d�9�v�����|^�v�Z�F�^�z)�����&]&�h�f�Jx衇ЧO�n�(@˚ꐆ�)��$��qF�󹦈�:��o��-Z��^P�`�@�I������?��O`X͚��-Z�檪VT`��g�y��~{UeZ�[�gr��o߾�પ�j�Տ���Zek�
p��#�<����d�,U	0�1��`��z}d��N���ٸX��T������ȑ#++�z��)@�{����UVu� ��9Mq@�`M��?�>}z�a�*��3g6��Y[�(@.����C��Jf)Խ��ȅ��Q��s�ԩrq� sDp��ĉ�W�5W�P�*���9��B��=����򪗆[+�>��*-/�U.�	�Ą��9�pV��֜�P�L�2� �~q� �	{�=�YYS� �b���S�9�P}Ȑ!j����I�.�9M�L9Ό��XS� Uiy�l���S��XS� �n/�*0}^��q�����fk�g7�㼽�J��&ǌ�]��n���q͜�� L��+뭩�Q�n��)}5c����W�Mf�!$7�r�&�R����E�� ̨��Svq!�/��]=���lso-π�p�G��m`MRZ�M�_�~\����q��D��������@���La!�+.Vb���K������֓�KH�7�P���0]_���p�:--l��&Up�TUBa�$��pfz�x��С��l�-'��==�9������tD_�f��)�ۤ6X2s��$��bn���&���Hl��~����7�oKOY�Z��|�Q1IX��%�TY�Կtq��`�h޼����� �k��Ex;�<^�9�M)W�Ϙ<y�]�\���!�����l��<���-��؂�{1�Q��ï$b�W�q�t�"��w	}�����e�rs���3X��!�ť�w�UȒ�S=S)s�^N"W7%券�B�p���̒BlM�ŉ���J��@p�7�TY�M����8�}M��T\������¡��b���g�����)��p@;o���� ����M��]����[��gf���m��hm�AA�GdT�$��JܱcG�6L)��/\�e+��n���%H��\8�Xv2�錁n�7H�~ꈢC�:2dp|��	����ݡN	Q���[{��������T�7�qc��
�s���89�$3+�ځ����9�\ O����J�_M�`�&��Ӭ=��`�ת�: �	'���L�r���|ZϒA��wC\�{Nq>�y�*�ʇ�F#":���ѭ��l V;{[Y���ss
p^�]K͔<�Ѿm%F32r(���ݝ�m�Ւp-g/^Umܯ#�ݜ�%�������md�(~�hgo�o� S��H����4���E��v&Rҳ����4�3�A�/'峲���u�f��&�.{$&e���A��6E`Ǡ\~��9.`��Ò?hb��H����h���ڤp���h{�.\GZqf&`�w;D��tTDw�p�j,�l��A���?33����u�b���8aX�NB��	���C� ���H|�~7.��Hy6pwn��fF��~x�������m[�/��e|�� 6�8�d��wj�Y�pE�?������r�1kr�.s�:ǃ�f�?.>_���i�o�e����]�X�a?�2���/�p%2I�C�CF�xoj瀥Kf����Y�O���A ������������kh(�O}�lSe��3�UJ�GMVN�9���6�D7����3=�|n��Avl�g�u����H��|q4��N��d�I��Wmn�}���g�@45=�������������qx��/������p9,�m9��\���1s� ����|�~��b+��&�����C'�����Ȓv��¢�����I)��x�/�"ẞuȳg�N�Ч=V�w/£�U*R::�W;\��E_/\"y�2�� O���;�4q��нK����1�P��o���O���N/��V�1�{�*0s�"�K��w�/���5��_�N=�}�G��4�z9{�mkW�U�.�X΄��#&���3N�e��v�Zb��*�D�ܱ��]�������<��r�3�������Ŏ���A]�=����Z��:g� ����w>ق��n�KOOC+O�61���rO�m��\p�H�Ν|��?�s��}�:n����;'Cð�`�p�-��2�6�<�A��㉅�U��ED��u&�*�$�CM���9?/<�p��4�>�·�Z���VD�&Mt�W�_�,�S;Y��es����X��k;Ǣ��ќV�ZxOꒇ�f5k�C�UǑX�q�Ъ�m�ڢT����<�d���}{w�ǯ?����װX�o�I<<oF��F>�4qt���]0��`L�i�3Vo�;��%�&w5S�A����4��c�҃�.���:�#}���Z�s�� �����~��\��>iEzS������sG )!G���R����"0npwth�#���16R��~[Sѕ���Vz��{MѿZ㌏M�"n����[�`�g+1�m-����W��¾)z:y�_D��-;T6�=�F�Fhx�^���݃C�/!7� }D��y#B8"M�-�u���~�Oצ�� =��~-��5�^]��t���iG�B:<�m_u��|�P�}2<�2I{�6}��02ĕb
��1�fL�ō"juа,�mjU��f_G'̓�V�D��ǅླྀP��� �@'F��(RA����H���[���/w*nl�����)cz��(��c_� qNr."�K�L�-[��N��8�(͚��ڛ9�R)6�[�����t�di����/�"�w ��9':�nC��ɬ gʤB���^�n�Jwѻw��:1\@��C���Čf�pK�����'��#{ *6g�E�ؙp�e���e�E���E�����"5�[����'Q�_��.�X��7��?Su,��I�=;��GKow���$P��j�7����nZ#u�0j�v��c�(�ɑ�&Q,N"�����½�%&��j��5�� ���>'��=	��y Fx�D����Î_B���v�ݣ���kSR���X��<��$�?퐑U~��R�5dJ(��HT_)�����&n��_i�=+;���*��l�K����9Ao����E=l�h5O�_m>$�HQ=\"e���5��\Js�lMÒ�z
_&�㻔�X{OG���W���TXu@"^�E���GǛ��R�b���77��/~�7�-Ԯ���͇�os4�rþc��(=�E�w�3�����Y����6$$��u$����#8"V��E��^1zn��D�4�� ���r8}^Z�ƉRdδ�������w��m��ҩ�q�Z�f\�I����w����dCJ�D�2�V�锄w���|N:v	�z�_VLSV���P� �8��,30Y�;�c�oW�w�ָ�De":)�E1��롸0"*	���G�6B_,�>�����R�s:!1+���|]^����JM�°�]��1X�q��\�pq��y���=�h"����8-M�Q>�5)�Q(����T���%(�1ɑ��-;ѥ��戔(�2:���,g'�H�	�A�HZմ�iq3��/ �%n}<$
�G��vC�\�?>m�Z�6J\�ŗ=�:�!z���P��
��j�뎼�ɄP	W:���쓢���V� O&"�p8+�Ģ���קY2�7�pc�}8|<?��qѿk7���#�]4Z��**:[��ž��ر;���p]�b���x)Qb���}{�������/\AX�5|Xt�h�*~�aqxp�p\��7�E��K?�؉�R�þ#���ظT���E"_ǃ� E����dK�m�n2����p(��6�m���i���O�X,ƒ�&�0i�U�rVV�z�Ʀ�`�/���[���C�� 7ȵ%v��	x�}6�vq�<ѻ���X*n�֔hlN�FZ���j0�t>�<ay�x/�<F����ŗ=$n��%�0@,�Xថ�12$J�O�y�� &F��x��8�H�n�s;��`@�@�i���w�\tZ��(+�A�?>>Yf[���$'���D�x,�+���X٢�[4���;�c�Y�&\a�8:��8�~�b�^�|qa�pf+�zf�D����1�J
0(����T���Մa���Zm��X� S=��PXX�bmS �N���^&��pT,d��$��|o"�X�V)a� �P塌k7�q3�5�[��"#ꚋ��{�R��a*˧�L��0Q/OL3.���� �5b��>�W��mZ7�����.^A��.1r���Q�;J��s�ڠե;�~�7�)O�I�J�gئ��C��U�9�oW�]����3I� f�QQQ��M����'�j$�������pҁ�]�@~ҹ*�4������xEO'�3���bxL�lo;��U^��8�!�k�jP��{`��|_���揆�L^H넛sD�����g�P��~}y��D�Q����3,C����R����{��d�E{y?q�LT�o���]�`�O��D�ʕ+���S�q����(��an>jE�a�f��tI������e:���$�x|.�S���n�����+X�W��Cv�wV~�g/DIH�QY��bD���	�W�(�H��m��\�6BhL���M���6���M���{�/))I�+��:� m�����/MMtZ	�>ؼ��(^������|V��tSE�:8�	����E���u�h�o���W;?-�G�Q���Т�)U]d��O��r�����v�&d$��I(���sX�<���Z�3Cm��2��{��Y��h;uvr�[���ֲ�2	��RL'J=Ƥ����(F�4a�~�[B��T[�7Nؿ��!�=!�Qz�+�U |��9�2 �|�om���#F����3N�&��ok[���wW�����)����ܹs|��)�V(�J�&β4ւ�2k�wa��ŋz�KQ�:"!!����Y{�(K{�_�c�j��	nNN���/0)�p%�AoM��4��2j�8F)���D������w��u��N3�H�R ���˗/����55
?~\E�ȨZ*0�'�|�55.
|��7zݫ�������G��#j��ǆK����r�������]��]���A�~�A�UC�̖�0�0�h�њ>8�O�4t��V��vq͚5*>���&�K~�[�^!���X��ab�o�^�ܹS�L�%��?��3���'g���M�6U��Q�̮Pa�ٳ�O7<25���ٲe�apð7�L�)99���z���F���� ��~�i��nT
0�@.�[/�\\���`�ʕj���kd��J����aZ�~}���Vh�n
Ѕ���o�\,P%�l&޸q�u��<�Ժ�n�������{YQ� ����u�Ԏj��㏪� ̒�s)�'�|bu�j�O��f���?V���^�«���M-�W�V��v�z�;
p��G}�Teu7�5	`�j.�~���ՌS�u�Z)@Ѽv��
��QI��Yy�;G.�y�&���pyHy����C���>��r��h�M5� 3��m][�ʬ�jF>g�d�5�g*CU(��#�q��˗�		�K֯f� �^�bv��]#iY#��Zp���x���S�f�ذa>����V��G���2#*O=�h�[�y)���_+���(w��:������������"S����r�!g���է g�^|�E�I��f��SZ���`!�xV��C���p��3�<#����H��\k���(�CBB�^��dY��iG���/(���{��h6�Y(�5�'>3|�p�g�ae��S`���X�x1���M
fTV�Y��&'��T~z���QY��d?-YW���c�ҥjj����լ �PM'sC�cǎ��@X_t�������}��W�c'�2ʖX���֊���B����=z��rrZ���r�Q�����?�]��j�
UF/��J	2''�o߮��>}�X��h�&Էt�(�H+K$��S'S�p�C�ɦ_�Ѷm[�u�D2g��+s�ܲe��V�`K%�̆k�366۶mS�w���7���|����3���y3K~��6�eR��o�{w���mՏ>�(ƌc�S��Y�F��U��AnnC����9�ZchT��M� u�
h�ڭrd��|n��J�ڄkB�z���~���)S�(��ҥ�ٜ����p#P>���~����ZK�ڊ�]� k��#���3f`����ݻw���t{>�u�֩yr��\[�j�m k�!�4><==1s�L�=Z����wCMlspp�z��.7A�Q\}�Z#9򩣹�f@@�z�1�A���e��#C��3��g��N�.auW;�U�$�Z��ܠ�"�⛜=x�`L�0AAA�j8w%�IXK&J�UZ���ٺu+.\��vt�~e�um<U��`�Nl~�ė/җ��0(�8����:�4`��0�o|,�G���,�&� ���>��F�ʎ�`�N���\���С��<p�Ihr���IPh�s�v?9��q�F�8�=��q��1nKC��h6&*��.��sG3z41�Lp���yy��3��t)���q����@D�v<����\��n�����>�ꑣ    IEND�B`�
```

### `src/shared/assets/logos/ttml.png`

```tsx
�PNG

   IHDR   x   x   9d6�   gAMA  ���a    cHRM  z&  ��  �   ��  u0  �`  :�  p��Q<   DeXIfMM *    �i            �       �       x�       x    �U�  �iTXtXML:com.adobe.xmp     <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 6.0.0">
   <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
      <rdf:Description rdf:about=""
            xmlns:exif="http://ns.adobe.com/exif/1.0/">
         <exif:ColorSpace>1</exif:ColorSpace>
         <exif:PixelXDimension>150</exif:PixelXDimension>
         <exif:PixelYDimension>150</exif:PixelYDimension>
      </rdf:Description>
   </rdf:RDF>
</x:xmpmeta>
��Q�  bIDATx�y����/�B�������81踠�w�(�xN0�	�瘃�0IԘ9'�$�9�x��$L�}\X�Mh�ek�����e������_���^�z�uC_-�uU���}����U�4�YF������ ���kv���\�ܽ{w�l��vֿ;kŭz`]]�9����唔�ٳ�dffJzz�$''K�=$11� �C<{��������)++3Gee���<�a�o�ٙΝ`�� ǎ+S�N��#GJ�>}$##C���k~gee	�����"}uu�9***���D�������R^^.����������3$%%IBBBs'�@w�"��4("t���r�UW�̙3eȐ!���l�W�N� �q��Iٱc����;�y�f��V9]�wX�-.�c�ҡC��W\!�\s�>܈_Do,	�F��U�d�֭�� �H�e}���� �s����F����;On����K�xt�R�z�ĉ�v�Zy뭷��/����:ƺ�z�0
c��W_-��͓�.�H������"��>�5k�G�pě�0�8F�3g��~��FaB9�TPP 6l��{Nv���lv1Vǃ�0�b�`� �y��6m�:�u�(g������69��k���,�k�ĉ�h�"��댭뗏Ey��?��r�J9~��1�b��1�q�����s�J���c��q/c˖-��/��ի����3���gϖ����2}���7z�+������]~����ݻ[y֢U���x�ԡ"?�я���3�n�^�3����_�B>��C�`F�����e�p��Ǐ�+V�w�aƠh��Y���C��>�|��(��Q�禛n2"	M��ZZ �p֬YƏ�s�N9r�HT@���XL����~�3�+ny��_�-�����۷{nJy����x�cP��s�(t0Q��ɷ�~+���ov��N�쮧 3�����ŋ������7:{����2�v<a{��5/ᅽ���e*�|�r���;g+ǹ�H��/�\:شi�'��	�p.�>���r�-�Ĺ�:w�(_^x��۷��ʼM$�1��K��-[f̠H*ӹ����͘1C���cĵ��_�����;����^3��ˆ���W������KpK���x�t�Ҩ�pn_�lI���/���o��!� 3#�B�ƌY�E�i�q���c�=f&e`�p�����_�Z����-���0[�6��?��s�g(��-��	�	&�Yծ�ݶ ��HL�+
`K��?��n���g#k��x�	�4i���s�[X ��`f��z���(�-��Eۧ��Q��t� ý�gK�,��:ɻ�(� �HP���c�ɐ�?�ϻ(~-�S�{���\G�#����x���5��ף����5�\C2u6bA �1��^�ۍ�$��Ȉ#:l[(nRQS'GOUˉ�Z9]]'��r��^�h�z�!�Sa���N����]�{$H��$�LI�T��+%I��I��ɒ��x����?��	�'�>���.�׬�c�W��ګ�W��:�+����5rD-,��o���w'���t�T�������k�3�ʽ���*n@�;b:)�CA�s��dث���M��~�2�O�dg�H��d���$�e�bBQH��^�e���/^��yX�^X&;���#r⴮�>#յ�Ry�&�����V�����v;"x�LMNP.O��i�ٽSd�О2-?KFH��	2�^<��k���^zI>��ӠVMȐ̢1cƘV�XJRPT!�
�çe�����X����E�ʑf,U0!hGz���7���]$A��Pq���`���3�djN/#C��֭[gB�p@���v=�wc	n��z�{�\���H6|["{��K���J�RK�� e�6Q�]"4h����Jt��^�h���q;GE�Լ^r��~rш>�������/sǗ]v���ƌ���Lc2����o���	���B�ͭNɛ[�Ȧ��d��
)WE���ġ�����$DL�D�u��}��rդ�2X�m8>�������r0�m�E{iI�j���ʛ���;_�P��Jjt<5B1���I�ۅH�*v(zGU_����rL�L(���'cexR^�LXf���o��=�f�e��믿>����p���n;X&/}rP���H��V�q�����q���(z�*�?�u�����e��rd��A�O9�k���3Q�0Ø}h8]��0��;��r�j����w�pO�X��3�i'�Z' Y�\[ר]m:s��J��U嬻�b��pJYeSπ̍K/�Tz���OOi��2�/�IY�o������K8̌�
Gc�S����òq�I�u�P�wV��J��Khy��[LYVI�nc����A���$�PP��򕼶�U��D�Y±����N�`�
yn�>yf�.���"�a�؟� �=�.6DlxE�W��+?y�l�[�vl���s���S���_����nͤ��W��d�^�m##���j�|����>�R��}>`o^	M�P��)a��-ŭ�U�_$�����I�DM?�����<�gtl~g��(�(eWN�2��d([,�Bm[̋Z��-����R���*�譀�=����~��v�62��x��L�	zQ�x���o}�5�<�Q�?�Ӱ\���|Д�5��}>l4~5,�5��9�w�W�.�����4٪&��v��̑}�$�QW^y�� ��&( �\pA�L�ިQ'����T��`��`������L���8M}�iɉ������}w�I��L��'�葤~�n��iz$&��":-c�+
e��4,D��]���O�u�C�D`�d��g¢B;F�r�����w��U����j�|�����5c�0�J�>�}��)���#���������ɛ��duw�%�/��%R+��8''Gz������t/k�>��>�n�e�� ����$u�,��ډ46�vc���`��OK�4�Sv%�='&±>�j5*2����<p~�4Y�:@��h�D�k�^4T�^;��m�]�35գ�p/��(g�5�Q�l���3F*�W1%Y��X5:UY#�tf����������v�i���ⴃ���7e�ߞby��]��wL��}݇!���${n"�� �W$Rڡ����e*����F_�e�����!����)��3��=UL�V �*����d�p�&sO��}ń2L����t.�ָ~���37�5�ێj�ߵ7WkȳJ9�ɐ��3rRA/.���.���:���Cŕ��x��<Ǆ	��q�S>�yB��^�,�3Jz黻!<�X@l��&�H�	�����]^�4���X#*���(�ʙp$32�ʍ��}㨛�r�����T��pL���n^��'*e�R�� <l�p�b��X1����Ҁ}P�?���dy��u�a��*g���ԕ�P ��S� K	V�M���߮+#L�#�4b"Eف��`�q$�:I���C1��?v8�$�g28D�޻�v��!vZn��ϱ4N�vaq�|��1c;�=zG��8�����
Φdq��tE���Ly�Ʊ��/�;����Js�vc���Ό���}3{����`��J�8E��E��Q�����{��u�a���R �<��A8����0��"t~^o�{�o��:�[b�l+^��V L���lD��$i�t����V����j��(Ph��:n��fJ���z�2��*�Ec��i�gTfn ��435Y�ɔ�.&���X�P�>e�D�k6���3�S���%�����t�����3'��zb���z��Ct@LOQ�^%d�>L����8"! �Qٽ���g�T�N�F����d*!��R��2c@�+��:>6܊�A:ܛڶF�d{�,��-dt��9��@^����uێ�S�)EZ��U8�V=�s"\͐��n���
f��Rd��y�f?�w��S�Y$�����O�a,!��n0>h��B�����e5��NMD\��
9��KU��nD�2���H�c7��(��h��b�}���a�|� ��MQ@�}�RK.����ֻn�Q�$¡m*⟠�o޿A;4u�����Έ�	F��M'@�+�puZP:�LS��	�K2~��ă�KD��Y:t��n�����_��;(b���:V*r�7lQړ�Lf�F���/ct�J�@D=,2�i�Ӻl�
��JSWd���bM}��-��:-�H5�c��"�21��&�7��SG��T_p�8���c��p*�0�n3����vK�*V�Hx--g��-���n�#]K��D�{�x"�S��[iݞ[����;t
�_�:#��j���C�e�Cd�Fp�C�s�i�~�v!7�c,��Ms���1�U[!IEAVZ�	�h3���ßC{)����g�F�6�7̀r�mQ��r����6h��K��*��vUo�M�:
�&l7��ʆu�gK��`���tC��`.XSL�fX�c
���?����ը��nfnbZNm�W�R3Z�'�$�'Ȋ�Q>��sC�� �ɤ#��[���Ȁ��u٨	�V 6�֧� �{�`���"΄X��e�1��X�m�?�!,G�'S;ާ��R�P3*����'�c�g��F?����=��>fp`/����.��q(��H��x�v��|�� 3;]5�_ ����Ԡ�����owRN(�[ �;�qh����W|�~0θ!��'T˶����-���@��niB�9�KX�|s�+�����0y�J���E�#�(|n��x��l%F��Smn����fF'^��!�Y�`�2ʋ��uF`78����u�&Q���kf��-F��P+Ͷ�ͺ��:�9E�����X����\)��I���Xu�2�	�K�}:����w�ɡ��R3�Ԋ��A����=q��>s��*�G�����R����m�G������b���h�zƗ���2�v��,��t�w��Q9u��k4s0"��"�������d.�`,�Ҙ��9�i��Z1iBU�o�k|����:�3�I?�QsC|��
�k0�p�IS&�|/�x�B5��J�J�R�f�F��"kn~&xDnӹZk�nN�tB�x�Yų���wӭD4�׹�"������-�ff:e�j��2Cѵ�,��������(��4v:6���B�[B<�Ƭ�d���GB�{�E��o�f�3�����D�T��
E8�-�o��u���<�	�mI������2�������:۪�*�) k^�YD[9~���O���j�?p�p�boT��J���%GCrqt��z�$���F���k��o^�J���.�r���FQ�do.��W_}��ӦEؓ8RBTS�{��+Y�+�yi�Aң^:DE[{�HcZԞk���tV�[I=;�9j-�S���kF�V��
;vL�-(c��H	���&��Ю�F��==��y�m�Q�)e_�c+`R"D�!t���^D�P�j}<�KhX()<x�(X����{��w#-Ǥǳ��쑲x�H�-`4�5�=��"��t�&��|�
�}��u����<��;�OǙw�`Y~�DS���Ać��+���Z�h��?�8H�_F�-��'�`���^����/b���Фћ�4q��~��7�W�G���1Ǘ�3�6_M��tH�8nǙ6=��˚${dl�V�U�>p�W���ݗ���['��Q}�}�Ȧ�pj�z�΅����luv��+#%:	A���R呹��ˬz���%c0�jQ�ao��Ỷ^��e���S7��[f1���$�A���F����DSjF��è�1��{�=j�s�ԳU
r��0 ��z�1�	Kn� I¾�����M�6�[�/|��}��YL�&E�~cB��'��+?>$�i�:+L$�����-���Gh��J�vp:V{c0y�o�Du4��e��B�u!��̻p�.cM5KZ}w�����،����� ���ޓ�o�9*���~��郄��ۏ˪�
e�.��D�S�Q�rU��Ε��p��!�ݩ���� �l[�F���~�7�	Q�W��\=y��t��7놥�ڙv����v�L}��>��#cOE����.����B����P>�]lH�+�Y��2�X��r��2M�s{Ƕ|+.�K��A����X���}���r��ih�S�ĺQ���2�7�8��Wћ֮]k�&��E�� �P��x��r������h�������_.�u[��;��.]DV�k�@��mXՇ�tB>��I^j#�[#4ܯ�D,l�FDK^�T3m9���f��P�HW�X�&�pn�� � \����}���`��nzFo���8���QX*����S|��-ѭ�Ϙ�������� ��S�Z�ŉ�DZ:��A�f�	�M��
�"e�7���:9q�.3��HbG�����v2(�p1v�ʕ+�g��E=M(5l��^i�@)�m6�έ�ƞ��E��E����l	�p�&�	��h�_�p��й )�gwL36�90� 
�t �����X��5k�hG\nP��$��|�M���H�v��uu�i0�i�kTQ9��zԕ����=g���N��p��o8hñŧ����b���(Sr�Lp=�� ��"^��7n4���!d��bf'�򗿘Ϫ� ��
k[��9�f2�tu�6v� �@�b�,��9���������Kݚ	c-��&��6���.�����kr�]w��7��%��c*�ȉidO˦gK�e>��"�7���zG"�l�c�-����裏
_�E�(�-PPP`��SXXt����Gf��#�-�6m���g<Z �����m���<�����V?ģ1��2�n�jLX��x0��]�F͠�E�k������~gf�B)WVq��0v#\�m�6�R�9�-���/�F����*a�$r���W@�D�5g-�߳�>k�r{���1,�I�󃙦U�V�mz�����m���(�@0��*P~V'Ѓ�5z�1�	�V�=\V=Υ3f�ҥK�hv2���&l&1�0�/[��Y�3���}�^��8��P�pJp0�=ٻw�q�D��8�ʞ��2����O��h��*lm%䌸f6n�5kVH��=]�og-����p߾}��6b�1��l�b.>u�/�쵺��h�E����H�4"��\́�(>	I���\'���'?1Q�n�1���p2� yƌ] ��Ĥ/^��X)N����`d�p2f ��҇��p���G/���<���df:6l�`B8�M�&|����o��z�0�W�R�� �!�<@��|���2e��i�uQ�`F��%K���%���9�d
��p3_S㋦]Ժ�.�yݧ�zʄFEC9�
�� s0������G�n^���UϽ���x��'�^0��/��V��V�T��[֭�ڵ����>ӹJX̫3��׺�P9m?G1YN3�c/���/>��Y���x���p���òb�
���x��[�Y��3��@��x�7o�Y5����r:ҙ�oX�\.R��bQǘl�S`��.�9s�E̺w6�����W_5�N�b�����f]p�رc��[o����oߞ ��'�?��(����[��ǁ�-� S!�e�f<Ɣ������o�A�EU���^C��u��5�׍��줾qت$@s������/7b<�/�ZeD��� x���3��l���0 �+	Ќ�|dq���r��S�)<�Mj�O޼y��v��m�w��?�v��w��z1��q�oL,&1�l\��}6��M�|-�, Ö�h�Zmء�*	g0N8��Cd�EL,"I&O�,���fg,�ľF�cʓ�����8��T�O֎���8֝�j��N��e ���=@��B#���/p�Yx� ���d�,�-�0�H���ت��# ��Q�V{�B��t { ��0~s�Ɂ�b7U .E!h� ��䈦1�{x}��8T� �?�����9T��^���������Ī�#�����"ƭc��    IEND�B`�
```

### `src/shared/copy.ts`

```ts
/**
 * UI copy. Every user-facing string of a flow lives in ONE file, `src/flows/<flow-id>/copy.json` — flat keys
 * ("empty.title") → the text. Screens read it through the flow's `t`:
 *
 *   t('empty.title')                       → "No search results found"
 *   t('group.results', { list, count })    → "Stocks (8)"   ({name} placeholders are filled in)
 *
 * Sample data (stock names, prices, watchlists) stays in the flow's data.ts — copy.json is only the words the app
 * itself says: titles, labels, buttons, empty states, accessible names. Porting a flow: this file is the string
 * table (Flutter ARB / iOS Localizable.strings).
 */
export type CopyVars = Record<string, string | number>

export function makeCopy<Copy extends Record<string, string>>(flow: string, copy: Copy) {
  return function t(key: keyof Copy & string, vars?: CopyVars): string {
    const template = copy[key] ?? key
    return vars ? template.replace(/\{(\w+)\}/g, (m, name: string) => (name in vars ? String(vars[name]) : m)) : template
  }
}
```

### `src/shared/data/broker.ts`

```ts
// The broker behind the Lemonn app — shown wherever the app needs its regulatory line (Stocks home, Profile).

/** The broker's regulatory line, in full. */
export const disclosure =
  'The online broking and mutual fund execution services are provided by NU Investors Technologies Pvt. Ltd. | CIN U67200MH2021PTC364704 | SEBI Registration No. INZ000304837 | SEBI DP Registration No. IN-DP-712-2022 | SEBI Research Analyst Registration No. INH000016764 | NSE member code 90251.'
```

### `src/shared/data/format.ts`

```ts
/** ₹ with Indian grouping: inr(123456.5) → "₹1,23,456.50"; inr(1000, 0) → "₹1,000". */
export const inr = (n: number, decimals = 2) =>
  '₹' + n.toLocaleString('en-IN', { minimumFractionDigits: decimals, maximumFractionDigits: decimals })

/** "9876543210" → "98765 43210" (how Indian mobile numbers are read out). */
export const phone = (digits: string) => (digits.length > 5 ? `${digits.slice(0, 5)} ${digits.slice(5)}` : digits)

/** Keep the last `visible` characters: mask('ABCPM1234K', 1, 5) → "ABCPM****K". */
export const mask = (s: string, keepStart: number, keepEnd: number) =>
  s.slice(0, keepStart) + '•'.repeat(Math.max(0, s.length - keepStart - keepEnd)) + s.slice(s.length - keepEnd)
```

### `src/shared/data/market.ts`

```ts
// Market data more than one flow uses (Stocks home, Market, F&O). Sample values, from the live app on 2026-10-17
// (indices and today's top gainers updated from the live app on 2026-10-22).
import asahiindiaLogo from '../assets/logos/asahiindia.png'
import crisilLogo from '../assets/logos/crisil.png'
import hoecLogo from '../assets/logos/hoec.png'
import igilLogo from '../assets/logos/igil.png'
import jindrillLogo from '../assets/logos/jindrill.png'
import jublingreaLogo from '../assets/logos/jublingrea.png'
import oilLogo from '../assets/logos/oil.png'
import ongcLogo from '../assets/logos/ongc.png'
import relianceLogo from '../assets/logos/reliance.png'
import sagilityLogo from '../assets/logos/sagility.png'
import ttmlLogo from '../assets/logos/ttml.png'

export type MarketIndex = { id: string; name: string; value: number; changePct: number }

export const indices: MarketIndex[] = [
  { id: 'nifty-50', name: 'NIFTY 50', value: 22687.65, changePct: -0.54 },
  { id: 'sensex', name: 'SENSEX', value: 73249.39, changePct: -0.62 },
  { id: 'nifty-bank', name: 'NIFTY BANK', value: 49812.4, changePct: -0.21 },
]

/**
 * A stock as lists show it. `logoUrl` comes from the API (the company logo); until it loads — and in this prototype —
 * the L3 Avatar shows `initials`. Logos in src/shared/assets/logos are cut from the live app's screens (prototype only).
 */
export type Stock = { symbol: string; name: string; price: number; changePct: number; initials: string; logoUrl?: string }

const stock = (symbol: string, name: string, price: number, changePct: number, initials: string, logoUrl?: string): Stock => ({ symbol, name, price, changePct, initials, logoUrl })

export const stocks = {
  HBLENGINE: stock('HBLENGINE', 'HBL Engineering', 833.15, 17.27, 'HB'),
  SHRIPISTON: stock('SHRIPISTON', 'Shriram Pistons & Rings', 4629.9, 6.85, 'SP'),
  THANGAMAYL: stock('THANGAMAYL', 'Thangamayil Jewellery', 5098.05, 6.41, 'TJ'),
  VAML: stock('VAML', 'Vedanta Aluminium', 371.85, 3.42, 'VA'),
  WELCORP: stock('WELCORP', 'Welspun Corp', 2571.5, 3.4, 'WC'),
  LINDEINDIA: stock('LINDEINDIA', 'Linde India', 6184.55, 3.11, 'LI'),
  IEX: stock('IEX', 'Indian Energy Exchange', 139.42, -6.12, 'IE'),
  ZOMATO: stock('ZOMATO', 'Zomato', 218.6, -4.85, 'ZO'),
  PAYTM: stock('PAYTM', 'One 97 Communications', 712.3, -4.2, 'PT'),
  DELHIVERY: stock('DELHIVERY', 'Delhivery', 341.75, -3.64, 'DL'),
  NYKAA: stock('NYKAA', 'FSN E-Commerce (Nykaa)', 164.1, -3.08, 'NY'),
  POLICYBZR: stock('POLICYBZR', 'PB Fintech', 1532.4, -2.71, 'PB'),
  YESBANK: stock('YESBANK', 'Yes Bank', 18.42, 1.21, 'YB'),
  IDEA: stock('IDEA', 'Vodafone Idea', 8.12, -0.98, 'VI'),
  SUZLON: stock('SUZLON', 'Suzlon Energy', 54.2, 2.35, 'SZ'),
  IRFC: stock('IRFC', 'Indian Railway Finance', 128.65, 0.84, 'IR'),
  NHPC: stock('NHPC', 'NHPC', 78.9, 1.12, 'NH'),
  SOUTHBANK: stock('SOUTHBANK', 'South Indian Bank', 24.35, -0.41, 'SB'),
  NTPC: stock('NTPC', 'NTPC', 331.2, 0.62, 'NT'),
  COALINDIA: stock('COALINDIA', 'Coal India', 381.45, 0.45, 'CI'),
  HAL: stock('HAL', 'Hindustan Aeronautics', 4120.3, 1.08, 'HA'),
  ONGC: stock('ONGC', 'ONGC', 236.9, -0.33, 'ON', ongcLogo),
  SBIN: stock('SBIN', 'State Bank of India', 742.15, 0.27, 'SB'),
  JINDALSTEL: stock('JINDALSTEL', 'Jindal Steel & Power', 862.4, 1.46, 'JS'),
  COHANCE: stock('COHANCE', 'Cohance Lifesciences', 1084.6, 4.97, 'CL'),
  OIL: stock('OIL', 'Oil India', 412.75, 0.88, 'OI', oilLogo),
  IOC: stock('IOC', 'Indian Oil', 128.3, 0.36, 'IO'),
  BPCL: stock('BPCL', 'Bharat Petroleum', 279.55, -0.52, 'BP'),
  // Top gainers on the live app, 2026-10-22
  TTML: stock('TTML', 'Tata Teleservices (Maharashtra)', 38.01, 18.56, 'TT', ttmlLogo),
  IGIL: stock('IGIL', 'International Gemmological Institute', 353.2, 10.0, 'IG', igilLogo),
  CRISIL: stock('CRISIL', 'CRISIL', 4777.2, 4.16, 'CR', crisilLogo),
  SAGILITY: stock('SAGILITY', 'Sagility India', 45.39, 2.21, 'SA', sagilityLogo),
  ASAHIINDIA: stock('ASAHIINDIA', 'Asahi India Glass', 913.55, 2.18, 'AI', asahiindiaLogo),
  JUBLINGREA: stock('JUBLINGREA', 'Jubilant Ingrevia', 609.45, 2.09, 'JI', jublingreaLogo),
  // "Oil Price Dependents" basket
  JINDRILL: stock('JINDRILL', 'Jindal Drilling & Industries', 642.3, 1.12, 'JD', jindrillLogo),
  HINDOILEXP: stock('HINDOILEXP', 'Hindustan Oil Exploration', 171.85, -0.64, 'HO', hoecLogo),
  RELIANCE: stock('RELIANCE', 'Reliance Industries', 1302.4, 0.21, 'RE', relianceLogo),
  TVTODAY: stock('TVTODAY', 'TV Today Network', 168.9, 0.48, 'TV'),
} satisfies Record<string, Stock>
```

### `src/shared/data/portfolio.ts`

```ts
// The sample user's portfolio, shared by the Stocks home and Portfolio. FICTIONAL — never a real customer's holdings.

export type HoldingsSummary = {
  /** Current value of everything held, ₹. */
  current: number
  /** Overall return on what was invested, %. */
  returnPct: number
}

export const holdings: HoldingsSummary = { current: 24860.75, returnPct: -1.92 }

/** The user's watchlists (names are the user's own). */
export type Watchlist = { id: string; name: string; count: number }

export const watchlists: Watchlist[] = [
  { id: 'long-term', name: 'Long term', count: 12 },
  { id: 'banks', name: 'Banks', count: 6 },
  { id: 'to-research', name: 'To research', count: 0 },
]
```

### `src/shared/data/user.ts`

```ts
// The sample user every flow uses. FICTIONAL — the live screenshots contain a real customer's details; never copy them.
export type User = {
  name: string
  fatherName: string
  mobile: string
  email: string
  pan: string
  dob: string
  occupation: string
  maritalStatus: string
}

export const user: User = {
  name: 'Rohan Mehta',
  fatherName: 'Suresh Mehta',
  mobile: '9876543210',
  email: 'rohan.mehta@gmail.com',
  pan: 'ABCPM1234K',
  dob: '14 Aug 1996',
  occupation: 'Private sector',
  maritalStatus: 'Single',
}

export type BankAccount = { bank: string; accountNumber: string; ifsc: string; type: 'Savings' | 'Current'; branch: string }

export const bankAccount: BankAccount = {
  bank: 'HDFC Bank',
  accountNumber: '50100234567821',
  ifsc: 'HDFC0001234',
  type: 'Savings',
  branch: 'Andheri West, Mumbai',
}
```

### `src/shared/prototype.ts`

```ts
// Prototype behaviour the flows use — the ONLY way a flow touches the preview (src/preview). In a real app: interactive
// is always true, `go` is the app's navigation, and useAutoAdvance is "when the server answers, go on" (a loader /
// polling step).
import { createContext, useContext, useEffect } from 'react'

export type Preview = {
  /**
   * Prototype (true): real modal L3 BottomSheets, taps move to the next screen, loaders advance on their own.
   * Board (false): every screen frozen as in the Figma board — sheets are drawn as static surfaces.
   */
  interactive: boolean
  /** Go to another screen of the flow (by id). Does nothing on the board. */
  go: (id: string) => void
  /** The screen being shown — a new id gives the sheet a fresh mount while the page behind it stays put. */
  screenId?: string
}

export const PreviewContext = createContext<Preview>({ interactive: false, go: () => {} })
export const usePreview = () => useContext(PreviewContext)

/** Prototype only: after `ms`, go to `next` (loaders, "Verifying…"). */
export function useAutoAdvance(next: string | undefined, ms: number) {
  const { interactive, go } = usePreview()
  useEffect(() => {
    if (!interactive || !next) return
    const t = window.setTimeout(() => go(next), ms)
    return () => window.clearTimeout(t)
  }, [interactive, next, ms, go])
}

/** How long a loader or "Verifying…" state stays up in the prototype. */
export const LOADING_MS = 1600
```

### `src/shared/ui/MainNav.tsx`

```tsx
import { BottomNavbar } from '@l3/components/BottomNavbar'
import { NavIcon } from '@l3/components/BottomNavbar/NavIcon'

export type Section = 'stocks' | 'market' | 'portfolio' | 'mutualFund' | 'fno'

const items: { value: Section; label: string }[] = [
  { value: 'stocks', label: 'Stocks' },
  { value: 'market', label: 'Market' },
  { value: 'portfolio', label: 'Portfolio' },
  { value: 'mutualFund', label: 'Mutual Fund' },
  { value: 'fno', label: 'F&O' },
]

/** The Lemonn app's main bottom navbar (L3: Bottom Navbar). Every L1 screen uses this — never a lookalike. */
export function MainNav({ value, onChange }: { value: Section; onChange?: (value: Section) => void }) {
  return (
    <BottomNavbar
      aria-label="Main"
      value={value}
      onChange={(v) => onChange?.(v as Section)}
      items={items.map((i) => ({ ...i, icon: <NavIcon name={i.value} />, selectedIcon: <NavIcon name={i.value} selected /> }))}
    />
  )
}

/** The first screen of each tab that's built so far (prototype navigation). Tabs not built yet stay where they are. */
const tabScreens: Partial<Record<Section, string>> = { stocks: 'stocks-01', market: 'market-01' }

/** The navbar's onChange in a flow: `onTab={(t) => goToTab(go, t)}`. */
export const goToTab = (go: (id: string) => void, section: Section) => {
  const id = tabScreens[section]
  if (id) go(id)
}
```

### `src/shared/ui/Rail.module.css`

```css
/*
 * Sideways rail. Item width = the row (inside its 16 insets) shared by `--rail-visible` items with 12 between them, so
 * the same rail reads well from 360 to 412. --rail-visible is a count (set inline), not a size.
 */
.rail {
  /* Positioned, so absolutely placed bits inside (e.g. PriceChange's screen-reader text) are clipped by the rail
     instead of hanging off a far-away ancestor and making the whole page scroll sideways. */
  position: relative;
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: calc((100% - (var(--rail-visible) - 1) * var(--l3-spacing-12)) / var(--rail-visible));
  gap: var(--l3-spacing-12);
  overflow-x: auto;
  overscroll-behavior-x: contain;
  padding-inline: var(--l3-spacing-16);
  scroll-padding-inline: var(--l3-spacing-16);
  scroll-snap-type: x mandatory;
  scrollbar-width: none;
}

.rail::-webkit-scrollbar {
  display: none;
}

.rail > * {
  scroll-snap-align: start;
  min-width: 0;
}

.rail[data-snap='center'] > * {
  scroll-snap-align: center;
}

/* Two rows that scroll together. */
.rail[data-rows='2'] {
  grid-auto-flow: column;
  grid-template-rows: repeat(2, auto);
}
```

### `src/shared/ui/Rail.tsx`

```tsx
import type { CSSProperties, ReactNode } from 'react'
import styles from './Rail.module.css'

type RailProps = {
  children: ReactNode
  /** What the row holds, for screen readers ("Top gainers"). */
  label: string
  /** How many items show across the screen — a fraction (2.3) lets the next one peek in, so it reads as scrollable. */
  visible: number
  /** Two rows that scroll together (a grid of cards). */
  rows?: 1 | 2
  /** Snap each item to the start (lists) or the centre (one featured card at a time, neighbours peeking). */
  snap?: 'start' | 'center'
  /** Extra room inside the row (e.g. above the cards, for a tag sitting on a card's top edge). */
  className?: string
}

/**
 * A row that scrolls sideways — cards, baskets, ideas. Its first item lines up with the 16 page margin, items are
 * 12 apart and snap into place, and there's no scrollbar. Not L3 yet (a "carousel / rail" is missing there).
 */
export function Rail({ children, label, visible, rows = 1, snap = 'start', className }: RailProps) {
  return (
    <div className={[styles.rail, className].filter(Boolean).join(' ')} role="region" aria-label={label} data-rows={rows} data-snap={snap} style={{ '--rail-visible': visible } as CSSProperties}>
      {children}
    </div>
  )
}
```

### `src/shared/ui/Raised.module.css`

```css
/*
 * EXPERIMENT — every screen and sheet of the app, not in L3 yet: "raised" buttons, a soft 3D look like YouTube's
 * recent buttons. On when <html data-buttons="raised"> (the default; the prototype controls switch it to "flat" to
 * compare with plain L3). On top of the L3 Button (nothing in the design system changes):
 * - a faint vertical sheen: a touch lighter at the top, a touch darker at the bottom, under L3's hover/press layer;
 * - a 1px inner highlight on the top edge and a 1px inner shade on the bottom edge (the "lit from above" bevel);
 * - a small soft drop shadow, so the button sits slightly above the page.
 * Pressed: the drop shadow goes and the bevel turns inward (pushed in). Disabled / loading / ghost stay flat.
 * If it's approved for L3, these become Button tokens (e.g. button/raise-sheen, -highlight, -shade) per variant.
 */

:root[data-buttons='raised'] .raised :is(button, a)[data-variant][data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-10));
  --raise-sheen-bottom: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-5));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-60));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-10));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-10));
  background-image:
    linear-gradient(var(--btn-layer), var(--btn-layer)),
    linear-gradient(to bottom, var(--raise-sheen-top), var(--raise-sheen-bottom));
  box-shadow:
    inset 0 0 0 var(--btn-border-width) var(--btn-border),
    inset 0 1px 0 var(--raise-highlight),
    inset 0 -1px 0 var(--raise-shade),
    0 1px 2px var(--raise-drop),
    var(--l3-shadow-elevation-low);
}

/* Filled (primary, brand, buy, sell): a stronger sheen and bevel on the solid colour. */
/* (:not(ghost) only matches the base rule's specificity, so these values win.) */
:root[data-buttons='raised'] .raised :is(button, a):is([data-variant='primary'], [data-variant='brand'], [data-variant='buy'], [data-variant='sell'])[data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-20));
  --raise-sheen-bottom: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-15));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-25));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-30));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
}

/*
 * Secondary and tertiary: only a hint of the effect — no darker bottom (it read as grey), just a faint white light on
 * top, a soft top highlight and a small drop shadow (no elevation-low). The full effect stays on the filled buttons.
 */
:root[data-buttons='raised'] .raised :is(button, a):is([data-variant='secondary'], [data-variant='tertiary'])[data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-10));
  --raise-sheen-bottom: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-0));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-50));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-5));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-5));
  box-shadow:
    inset 0 0 0 var(--btn-border-width) var(--btn-border),
    inset 0 1px 0 var(--raise-highlight),
    inset 0 -1px 0 var(--raise-shade),
    0 1px 2px var(--raise-drop);
}

/* Dark themes: the light glint on dark outlined buttons is kept faint (a bright line would look like a border). */
:root[data-buttons='raised'][data-mode='dark'] .raised :is(button, a):is([data-variant='secondary'], [data-variant='tertiary'])[data-state='default']:not([data-variant='ghost']) {
  --raise-sheen-top: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-5));
  --raise-sheen-bottom: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-0));
  --raise-highlight: rgb(from var(--l3-static-white) r g b / var(--l3-opacity-5));
  --raise-shade: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
  --raise-drop: rgb(from var(--l3-static-black) r g b / var(--l3-opacity-20));
}

/* Pushed in while pressed: no drop shadow, the bevel turns inward. */
:root[data-buttons='raised'] .raised :is(button, a)[data-variant][data-state='default']:not([data-variant='ghost']):active {
  box-shadow:
    inset 0 0 0 var(--btn-border-width) var(--btn-border),
    inset 0 1px 2px var(--raise-shade);
}
```

### `src/shared/ui/Screen.module.css`

```css
/*
 * One phone screen: the OS status bar, then the page — top bar, a body that scrolls and fills the space, the bottom
 * bar / button dock — with an optional toast just above the bottom and a sheet over everything.
 */
.screen {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  overflow: clip; /* clip, not hidden: a sheet taking focus while it slides in can't scroll the screen */
  background: var(--l3-surface-default);
  color: var(--l3-content-primary);
}

.page {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 0;
}

.middle {
  position: relative;
  display: flex;
  flex: 1;
  min-height: 0;
}

/* Like a phone: no scrollbar that stays on screen — the page, rails and open sheets still scroll (touch, wheel, keys). */
.screen,
.screen * {
  scrollbar-width: none;
}

.screen ::-webkit-scrollbar {
  display: none;
}

.body {
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 0;
  overflow-x: hidden; /* a screen never scrolls sideways — only rails inside it do */
  overflow-y: auto;
}

/* Floating toast: sits on the bottom edge of the body, above the dock / navbar (L3 Aerobar rule). */
.toast {
  position: absolute;
  inset: auto 0 0;
}

/* The prototype's modal L3 sheet is portalled into this screen — lift it above the bottom bars. */
.screen.screen > [data-contained] {
  z-index: var(--app-layer-sheet);
}
```

### `src/shared/ui/Screen.tsx`

```tsx
import { Fragment, useState, type ReactNode } from 'react'
import { usePreview } from '../prototype'
import raised from './Raised.module.css'
import { SheetHost } from './Sheet'
import styles from './Screen.module.css'

type ScreenProps = {
  /** Actionbar (or nothing on full-bleed screens like the welcome screen). */
  top?: ReactNode
  /** Bottom navbar, button dock (ButtonGroup) or the keypad. */
  bottom?: ReactNode
  /** A floating Aerobar toast — shown just above `bottom`. */
  toast?: ReactNode
  /** A Sheet over the screen. */
  sheet?: ReactNode
  /** Screen content: scrolls when taller than the space between `top` and `bottom`. */
  children?: ReactNode
  /** A flow's own styling hook on the screen root (e.g. an experiment scoped to one flow). */
  className?: string
}

/**
 * The frame of every screen: top · body · bottom, a toast above the bottom, and a sheet over it all. It fills the
 * space between the OS status bar and the OS navigation bar (the preview draws those — real apps get them from the
 * OS, edge-to-edge insets), so nothing sits under the gesture handle. The screen is the sheet's container, so sheets
 * and their scrim stay in the app; its SheetHost keeps one sheet open while the screen's sheet changes (no scrim flash
 * between two sheets). Mark a bottom part with data-surface="primary" | "secondary" if the navigation bar should take
 * its colour (the button dock and the bottom navbar are recognised already). Buttons on every screen and its sheets
 * get the raised-button experiment (Raised.module.css) while <html data-buttons="raised">.
 */
export function Screen({ top, bottom, toast, sheet, children, className }: ScreenProps) {
  const [el, setEl] = useState<HTMLDivElement | null>(null)
  const { screenId } = usePreview()
  return (
    <div ref={setEl} className={[styles.screen, raised.raised, className].filter(Boolean).join(' ')}>
      <SheetHost container={el}>
        <div className={styles.page} data-part="page">
          {top}
          <div className={styles.middle}>
            <div className={styles.body}>{children}</div>
            {toast && <div className={styles.toast}>{toast}</div>}
          </div>
          {bottom}
        </div>
        <Fragment key={screenId}>{sheet}</Fragment>
      </SheetHost>
    </div>
  )
}
```

### `src/shared/ui/Sheet.module.css`

```css
/* Board only: the static sheet sits on the bottom edge of the screen, over the L3 Overlay — shown at rest, no fade-in. */
.scrim {
  z-index: var(--app-layer-scrim);
  transition: none;
}

.dock {
  position: absolute;
  inset: auto 0 0;
  z-index: var(--app-layer-sheet);
  display: flex;
  flex-direction: column;
  max-height: calc(100% - var(--l3-size-48));
}
```

### `src/shared/ui/Sheet.tsx`

```tsx
import { createContext, useCallback, useContext, useId, useLayoutEffect, useState, type ReactNode } from 'react'
import { BottomSheet, BottomSheetSurface } from '@l3/components/BottomSheet'
import { Overlay } from '@l3/components/Overlay'
import { usePreview } from '../prototype'
import styles from './Sheet.module.css'

/** Lets the L3 sheet play its exit before the next screen replaces it (≈ motion/duration-medium). */
const SHEET_EXIT_MS = 240

type SheetProps = {
  header?: ReactNode
  children?: ReactNode
  /** The button dock — a <ButtonGroup>. */
  footer?: ReactNode
  /** Id of the header's heading (BottomSheetHeader `headingId`). */
  labelledBy?: string
  /** Accessible name when there's no header (loaders). */
  label?: string
  /** Prototype: where a backdrop tap, drag down or Esc goes. Leave out for sheets that can't be dismissed (loaders). */
  onClose?: () => void
}

type Publish = (id: string, sheet: SheetProps | null) => void
const SheetHostContext = createContext<Publish | null>(null)

/**
 * An L3 bottom sheet over the screen.
 * Prototype: the real modal <BottomSheet> (focus trap, drag to close, scrim tap), drawn by the screen's SheetHost —
 * so when one sheet leads to another over the same page, the scrim stays and only the sheet's content changes.
 * Board: the same surface drawn statically with the L3 Overlay — dozens of open modals on one page would fight
 * over focus, so the board shows <BottomSheetSurface>, which is what the modal renders inside.
 */
export function Sheet(props: SheetProps) {
  const { interactive } = usePreview()
  const publish = useContext(SheetHostContext)
  const id = useId()
  const hosted = interactive && publish !== null

  // Hand the latest content to the host on every render; take it back when this sheet goes away.
  useLayoutEffect(() => { if (hosted) publish(id, props) })
  useLayoutEffect(() => (hosted ? () => publish(id, null) : undefined), [hosted, publish, id])

  if (hosted) return null
  const { header, children, footer, labelledBy, label } = props
  return (
    <>
      <Overlay open className={styles.scrim} />
      <div className={styles.dock} role="dialog" aria-labelledby={labelledBy} aria-label={label}>
        <BottomSheetSurface header={header} footer={footer}>{children}</BottomSheetSurface>
      </div>
    </>
  )
}

/**
 * Prototype: one modal L3 BottomSheet per screen, kept open while the screen's sheet changes (sheet → sheet swaps
 * the content in place, no scrim flash or slide). It closes with its exit animation when the screen has no sheet.
 */
export function SheetHost({ container, children }: { container: HTMLElement | null; children: ReactNode }) {
  const [shown, setShown] = useState<{ id: string; sheet: SheetProps; open: boolean } | null>(null)
  const publish = useCallback<Publish>((id, sheet) => {
    setShown((s) => (sheet ? { id, sheet, open: true } : s && s.id === id ? { ...s, open: false } : s))
  }, [])

  const close = () => {
    const onClose = shown?.sheet.onClose
    if (!onClose) return
    setShown((s) => s && { ...s, open: false })
    window.setTimeout(onClose, SHEET_EXIT_MS)
  }

  return (
    <SheetHostContext.Provider value={publish}>
      {children}
      {container && shown && (
        <BottomSheet
          open={shown.open}
          onClose={close}
          container={container}
          header={shown.sheet.header}
          footer={shown.sheet.footer}
          aria-labelledby={shown.sheet.labelledBy}
          aria-label={shown.sheet.label}
        >
          {shown.sheet.children}
        </BottomSheet>
      )}
    </SheetHostContext.Provider>
  )
}
```

### `src/shared/ui/Spot.module.css`

```css
/*
 * Illustration slot: a large icon on a soft accent disc with a faint outer ring. Stands in for artwork L3 doesn't have
 * yet (lemon mascot, PAN card, bank, coin …) — swap the whole <Spot> for the illustration when it exists.
 */
.spot {
  position: relative;
  display: inline-flex;
  flex: none;
  align-items: center;
  justify-content: center;
  border-radius: var(--l3-radius-full);
  background: var(--spot-surface);
  color: var(--spot-content);
}

.spot[data-size='md'] {
  width: var(--l3-size-64);
  height: var(--l3-size-64);
}

.spot[data-size='lg'] {
  width: var(--l3-size-96);
  height: var(--l3-size-96);
}

/* The halo: one hairline ring 12 outside the disc, in the same accent (lg only — the hero moments). */
.spot[data-size='lg']::after {
  content: '';
  position: absolute;
  inset: calc(-1 * var(--l3-spacing-12));
  border: 1px solid var(--spot-surface);
  border-radius: var(--l3-radius-full);
}

.glyph {
  width: var(--l3-size-32);
  height: var(--l3-size-32);
}

.spot[data-size='lg'] .glyph {
  width: var(--l3-size-48);
  height: var(--l3-size-48);
}

.spot[data-tone='brand'] { --spot-surface: var(--l3-surface-accent-brand-light); --spot-content: var(--l3-content-primary); }
.spot[data-tone='discover'] { --spot-surface: var(--l3-surface-accent-discover-light); --spot-content: var(--l3-content-accent-discover-default); }
.spot[data-tone='success'] { --spot-surface: var(--l3-surface-accent-success-light); --spot-content: var(--l3-content-accent-success-default); }
.spot[data-tone='warning'] { --spot-surface: var(--l3-surface-accent-warning-light); --spot-content: var(--l3-content-accent-warning-default); }
.spot[data-tone='error'] { --spot-surface: var(--l3-surface-accent-error-light); --spot-content: var(--l3-content-accent-error-default); }
.spot[data-tone='plain'] { --spot-surface: var(--l3-surface-default); --spot-content: var(--l3-content-primary); }
.spot[data-tone='neutral'] { --spot-surface: var(--l3-surface-secondary); --spot-content: var(--l3-content-primary); }
```

### `src/shared/ui/Spot.tsx`

```tsx
import { Icon } from '@l3/components/Icon'
import styles from './Spot.module.css'

export type SpotTone = 'brand' | 'discover' | 'success' | 'warning' | 'error' | 'neutral' | 'plain'

/**
 * ILLUSTRATION SLOT — a large icon on a soft accent disc (md 64 / lg 96 with a halo). Used where the app has artwork
 * (mascot, PAN card, bank, coin) until L3 has an illustration set; replace the <Spot> with the artwork 1:1.
 * Tone by meaning: brand = welcome moments, plain = on a brand panel, discover = info / in progress, success / warning / error = outcomes.
 */
export function Spot({ icon, tone = 'brand', size = 'lg' }: { icon: string; tone?: SpotTone; size?: 'md' | 'lg' }) {
  return (
    <span className={styles.spot} data-tone={tone} data-size={size} aria-hidden="true" data-illustration-slot="">
      <Icon icon={icon} className={styles.glyph} />
    </span>
  )
}
```
