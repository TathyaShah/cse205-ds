class Twitter {

    HashMap<Integer,ArrayList<Pair<Integer,Integer>>> posttweet ; // map<userid, < tweetId, time>>
    HashMap<Integer,Set<Integer>> followers ;
    int time;

    public Twitter() {
        posttweet = new HashMap<>();
        followers = new HashMap<>();
        time=0;
    }
    
    public void postTweet(int userId, int tweetId) {
        Pair<Integer,Integer> pair = new Pair<>(tweetId,time);
        ArrayList temp = posttweet.getOrDefault(userId,new ArrayList<>());
        temp.add(pair);
        posttweet.put(userId,temp);
        time++;
    }
    
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = followers.getOrDefault(userId,new HashSet<>());
        set.add(userId);
        list.addAll(set);
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i : list){
            if(posttweet.get(i)!=null)
                for(Pair<Integer,Integer> j : posttweet.get(i)){
                    map.put(j.getKey(),j.getValue());
                }
        }
        
        PriorityQueue<Map.Entry<Integer,Integer>> pq = new PriorityQueue<>(((x,y)->x.getValue()-y.getValue()));
        pq.addAll(map.entrySet());
        while(pq.size()>10){
            pq.poll();
        }
        List<Integer> ans = new ArrayList<>();
        while(pq.size()!=0){
            ans.add(pq.poll().getKey());
        }Collections.reverse(ans);
        return ans;
    }
    
    public void follow(int followerId, int followeeId) {
        Set<Integer> set = followers.getOrDefault(followerId, new HashSet<>());
        set.add(followeeId);
        followers.put(followerId,set);
    }
    
    public void unfollow(int followerId, int followeeId) {
        Set<Integer> set = followers.getOrDefault(followerId, new HashSet<>());
        if(set.contains(followeeId))
        set.remove(followeeId);
        followers.put(followerId,set);
    }
}