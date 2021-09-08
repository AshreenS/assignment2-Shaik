# assignment2-Shaik

# Ashreen

### My favourite place in the world is India!

I love India because it's the place were I was born.
Also, my **family**, my **pets** and my close friends live in India.
I live in **Hyderabad**, which is the capital of **Telangana**.

***

### Route to Overland Park from Maryville

1.  Start from Horizonwest apartments, Maryville and head west on w 11th st toward N college Dr.
    1. Turn left onto N colleg Dr then turn left onto w 9th st.
    2. turn right onto US-71 Bus S/N Main st and continue for 2.5 miles.
    3. Take the ramp to US-71 s and continue for 30miles.
1.  Now use the left lane to take the I-29 S/US-71 S exit toward US-59 S/Kansas city.
2.  Merge onto I-29 S/US-71 and keep left at the fork to continue on i-29 s
3.  Use the right 2 lanes to take exit 3B for I-635 s toward Kansas
4.  Use the middle 2 lanes to stay on I-635 s and keep right to stay on I-635 s
5.  Continue onto US-69 s and turn right onto W 80th st and then turn left onto Overland Park Dr.
6.  Turn left and you have reached your destination.
*   Items to be bought to Overland Park   
    *  Uno
    *  Basket ball
    *  cards
    *  Sequence game
    *  Business game

**[Get to know about me here](AboutMe.md)**



***

### Food Recommendations

Below is the list of **Drinks and Foods** people must try atleast once in their lifetime.
Also, the table contains information about where we can find them and the cost of the food.

| Food   | Location   | Amount |
| ----   | ---------  | ------ |
| Dum Chicken Biriyani| Hyderabad, India|500Rupees| 
| Jilebi | Hyderabad, India | 50Rupees |
| Chicago deep dish pizza | Giordano's, Chicago | 30$ |
|  churros |  Chicago downtown |  10$  |
| kerala parotta | Kerala, India | 100Rupees |


***
### Inspirational Quotes

>"How often have you wounde yourself by getting angry, fearful, jealous or vengeful?" - Joseph Murphy

>" Always be a little kinder than necessary." - James Barry

>"People who use time wiselyspend it on activities that advance their overall perpose in life." - John C.Maxwell


***
### Search for a pair of intersecting segments


> Given the nsegments on the plane. It is required to check if at least two of them intersect with each other. (If the answer is positive, then output this pair of intersecting segments; among several answers, it suffices to choose any of them.)
The naive solution algorithm is to search for O (n ^ 2)all pairs of segments and check for each pair whether they intersect or not. This article describes an algorithm with a runtime O (n\log n)that is based on the scanning principle (sweeping) direct (in English: "sweep line").


*pair of intersecting segments* description: <https://www.titanwolf.org/Network/Articles/Article?AID=8fa5a695-6dbb-4b9f-8577-64ac9163d9f1#gsc.tab=0>

```

const double EPS = 1E-9;

struct pt {
    double x, y;
};

struct seg {
    pt p, q;
    int id;

    double get_y(double x) const {
        if (abs(p.x - q.x) < EPS)
            return p.y;
        return p.y + (q.y - p.y) * (x - p.x) / (q.x - p.x);
    }
};

bool intersect1d(double l1, double r1, double l2, double r2) {
    if (l1 > r1)
        swap(l1, r1);
    if (l2 > r2)
        swap(l2, r2);
    return max(l1, l2) <= min(r1, r2) + EPS;
}

int vec(const pt& a, const pt& b, const pt& c) {
    double s = (b.x - a.x) * (c.y - a.y) - (b.y - a.y) * (c.x - a.x);
    return abs(s) < EPS ? 0 : s > 0 ? +1 : -1;
}

bool intersect(const seg& a, const seg& b)
{
    return intersect1d(a.p.x, a.q.x, b.p.x, b.q.x) &&
           intersect1d(a.p.y, a.q.y, b.p.y, b.q.y) &&
           vec(a.p, a.q, b.p) * vec(a.p, a.q, b.q) <= 0 &&
           vec(b.p, b.q, a.p) * vec(b.p, b.q, a.q) <= 0;
}

bool operator<(const seg& a, const seg& b)
{
    double x = max(min(a.p.x, a.q.x), min(b.p.x, b.q.x));
    return a.get_y(x) < b.get_y(x) - EPS;
}

struct event {
    double x;
    int tp, id;

    event() {}
    event(double x, int tp, int id) : x(x), tp(tp), id(id) {}

    bool operator<(const event& e) const {
        if (abs(x - e.x) > EPS)
            return x < e.x;
        return tp > e.tp;
    }
};

set<seg> s;
vector<set<seg>::iterator> where;

set<seg>::iterator prev(set<seg>::iterator it) {
    return it == s.begin() ? s.end() : --it;
}

set<seg>::iterator next(set<seg>::iterator it) {
    return ++it;
}

pair<int, int> solve(const vector<seg>& a) {
    int n = (int)a.size();
    vector<event> e;
    for (int i = 0; i < n; ++i) {
        e.push_back(event(min(a[i].p.x, a[i].q.x), +1, i));
        e.push_back(event(max(a[i].p.x, a[i].q.x), -1, i));
    }
    sort(e.begin(), e.end());

    s.clear();
    where.resize(a.size());
    for (size_t i = 0; i < e.size(); ++i) {
        int id = e[i].id;
        if (e[i].tp == +1) {
            set<seg>::iterator nxt = s.lower_bound(a[id]), prv = prev(nxt);
            if (nxt != s.end() && intersect(*nxt, a[id]))
                return make_pair(nxt->id, id);
            if (prv != s.end() && intersect(*prv, a[id]))
                return make_pair(prv->id, id);
            where[id] = s.insert(nxt, a[id]);
        } else {
            set<seg>::iterator nxt = next(where[id]), prv = prev(where[id]);
            if (nxt != s.end() && prv != s.end() && intersect(*nxt, *prv))
                return make_pair(prv->id, nxt->id);
            s.erase(where[id]);
        }
    }

    return make_pair(-1, -1);
}

```

*Search for a pair of intersecting segments*- source code <https://cp-algorithms.com/geometry/intersecting_segments.html>






